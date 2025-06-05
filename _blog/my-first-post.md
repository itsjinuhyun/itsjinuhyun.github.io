---
layout: default
title: Finetuning GPT-2
date: 2025-06-05
categories: [Personal, Technology]
---

# Finetuning GPT-2

*June 5, 2025*

## About GPT-2
Most transformer models are a variant of 3 main types of language models:

> 1. Encoder-only
> 2. Decoder-only
> 3. Encoder-decoder

GPT-2 is a decoder-only model, processing text from left to right, which makes it good for text generation: generating code, completing sentences, writing essays, etc...

Thus, it's good at generating text given a prompt while also being able to complete other NLP tasks like question-answering despite not being explicitly trained to.

Generally, language models are typically pre-trained on large amounts of text data in a self-supervised manner, then fine-tuned on a specific task. GPT-2's pre-training objective is based entirely on causal language modeling, which means the model reads the text linearly and has to predict the next word. (In practice, it's done by reading the whole sentence but using a mask inside the model to hide the future tokens at certain timesteps)

More specifically, 
> 1. GPT-2 uses byte pair encoding (BPE) to tokenize words and generate a token embedding
> 2. Positional encodings are added to the token embeddings to indicate the position of each token in the sequence
> 3. The input embeddings are passed through multiple decoder blocks to output some final hidden state
> 4. Within each decoder block, GPT-2 uses a masked self-attention layer, which means GPT-2 can't attend to future tokens. It is only allowed to attend to tokens on the lft
> 5. THe output of the decoder is passed to a language modeling head, which performs a linear transformation to convert the hidden states into logits
> 6. THe label is the next token in the sequence, which is created by shifting the logits on the right by one.
> 7. The cross-entropy loss is calculated between the shifted logits and the labels to outpupt the next most likely token


![GPT-2 Architecture](/assets/images/gpt-2.jpg)


## What makes GPT-2 special?

Released in 2019, it comes in several sizes:
- GPT-2 Small: 117M parameters
- GPT-2 Medium: 345M parameters
- GPT-2 Large: 774M parameters
- GPT-2 XL: 1.5B parameters

DistilGPT-2 (82M parameters) is a distilled version of GPT-2 Small, created by Hugging Face. It's 33% smaller than GPT-2 Small while retaining 97% of its language modeling capabilities. This makes it much faster and more efficient while still being powerful enough for many text generation tasks.

Given this, it's possible to fully finetune DistilGPT-2 (i.e. update all the model's parameters during training) without having to leverage parameter-efficient finetuning techniques like LoRA.

## Fine-Tuning DistilGPT-2 on ELI5 (Explain Like I'm Five)

The objective was to adapt DistilGPT-2 to the QA specific tone and domain knowledge found in the ELI5-Category dataset. This way, the model can specialize in explaining complex topics in simple terms, has learned the question-answer format common in the ELI5 subreddit, can generate more accurate explanations and has specific topic knowledge. 

Since the ELI5-Category dataset only contains 5000 examples, full finetuning is manageable.

## Technical Walkthrough

#### 1. Data Preparation
```python
data = load_dataset("eli5_category", split="train[:5000]")
data = data.train_test_split(test_size=0.2)

data = data.flatten() # Flatten nested structures for easier processing
```
#### 2. Tokenization
The most important step of data preprocessing is to convert the text into token IDs that the model can understand. Using the AutoTokenizer library, we can pull the tokenizer for distilgpt2 and use it to tokenize our ELI5 dataset. Then, we remove all text columns from our data since we only need the tokenized data fot training.
```python
# Load tokenizer
tokenizer = AutoTokenizer.from_pretrained("distilgpt2")

# Define preprocessing function
def preprocess_function(examples):
    return tokenizer([" ".join(x) for x in examples["answers.text"]])

# Apply tokenization
tokenized_eli5 = eli5.map(
    preprocess_function,
    batched=True,
    num_proc=4,
    remove_columns=eli5["train"].column_names,
)
```

#### 3. Handling Context Length
A key challenge was that some tokenized examples exceeded the model's maximum content length (1024 tokens). If we concatenate all tokenized sequences into a single long sequence, split it into uniform 128 token chunks, and create labels for the langauge model, then we can guarantee majority of the tokens are fed into the model and optimized for training on the GPU. 

```python
block_size = 128

def group_texts(examples):
    # Concatenate all texts
    concatenated_examples = {k: sum(examples[k], []) for k in examples.keys()}
    total_length = len(concatenated_examples[list(examples.keys())[0]])
    
    # Ensure length is a multiple of block_size
    if total_length >= block_size:
        total_length = (total_length // block_size) * block_size
    
    # Split into chunks of block_size
    result = {
        k: [t[i : i + block_size] for i in range(0, total_length, block_size)]
        for k, t in concatenated_examples.items()
    }
    result["labels"] = result["input_ids"].copy()
    return result
```
#### 4. Model Training
In order to actually finetune the model, we use the DatacollatorForLanguageModeling, which dynamically pads sequences to the batch's maximum length during training. This is much more efficient than pre-padding because we save memory. 

```python
# Load the model
model = AutoModelForCausalLM.from_pretrained("distilgpt2")

# Set up training arguments
training_args = TrainingArguments(
    output_dir="./results",
    evaluation_strategy="epoch",
    learning_rate=2e-5,
    weight_decay=0.01,
    num_train_epochs=3,
)

# Initialize data collator
data_collator = DataCollatorForLanguageModeling(
    tokenizer=tokenizer, 
    mlm=False
)

# Set up trainer
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=lm_datasets["train"],
    eval_dataset=lm_datasets["test"],
    data_collator=data_collator,
)

# Train the model
trainer.train()
```

#### 5. Inference
Use your finetuned model with the pipeline() function from Hugging Face. 
If your kernel is active, we can call it through trainer.model. Otherwise, set the model to the final checkpoint of the trained model. 
```python
# Create a text generation pipeline
generator = pipeline('text-generation', 
                     model=trainer.model, 
                     tokenizer=tokenizer)

# Generate an explanation
response = generator("Explain like I'm five: How does gravity work?")
```

## Key Takeaways

> 1. Data preprocessing is most critical. In our case, we flattened the nested structure, concatenated the sequences, and chunked it into fixed-size blocks.
> 2. Use the tokenizer that matches your model.
> 3. The fine-tuned model excels at ELI5-style explanations but won't have improved factual knowledge outside of its training data. Therefore, what's most important is aligning data preparation and training to the specific use case. 
> 4. Use similar 'concatenate-and-chunk' approach when training causal language models, working with long-form text where document boundaries aren't critical, or want to optimize for training efficiency on large text corpa. ELI5 approach, with the text-generation task, allows us to treat the text as one continuous stream. For other tasks, requirements differ.

For future work, I'd like to finetune GPT-2 for several other practical tasks, learning to accurately preprocess diverse datasets for niche usage cases. Then, I want to finetune an open-source Llama (7B+ parameters) with LoRA.