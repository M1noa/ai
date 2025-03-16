# Hack Club AI API Guide

Welcome to the boring guide to the Hack Club AI API! Here's how to make a super-stuid AI do your tasks across different languages.

## Table of Contents
- [Basics](#basics)
- [Understanding Prompts](#understanding-prompts)
- [Code Examples](#code-examples)
  - [Python](#python)
  - [Node.js](#nodejs)
  - [cURL](#curl)


## Basics

Talking to our AI friend is pretty straightforward. You send it messages in JSON format because apparently, each message needs two things:
- `role`: Who's talking ("system" or "user") - think of it as wearing different hats (you can have more than one)
- `content`: What you're actually saying as that role

### Response Format

When the AI responds, it sends back a JSON package that looks something like this (but with actual useful content instead of a dad joke...):

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "Here's one:\n\nWhat do you call a fake noodle?\n\nAn impasta!\n\nHope that made you laugh! Do you want to hear another one?",
        "role": "assistant"
      }
    }
  ],
  "created": 1742078489,
  "id": "chatcmpl-60751ce5-4b82-48e4-b7e6-caf933a92d04",
  "model": "llama-3.3-70b-versatile",
  "object": "chat.completion",
  "system_fingerprint": "fp_2ddfbb0da0",
  "x_groq": {
    "id": "req_01jpdzgc2deqr9p5cjtdzesxt0"
  }
}
```

Let's break down this response:
- `choices`: Where the AI's brilliant (or sometimes questionable) responses live
  - `message.content`: The words of wisdom
  - `message.role`: Always "assistant" (because that's just how it rolls)
  - `finish_reason`: Why it stopped talking (usually "stop", not "got distracted by a squirrel")
- `created`: When this masterpiece was born
- `id`: Its birth certificate (might not always be there)
- `model`: Which AI brain was used (might be shy and not show up)

## Understanding Prompts

### System Prompts
Think of system prompts as giving the AI its job description:
- Tell the AI what kind of personality to have (not really needed but it helps in some cases)
- Set some ground rules (like "no dad jokes"; but where's the fun in that?)
- Define how it should talk back to you

Here's an example of giving the AI its marching orders:
```json
{
  "role": "system",
  "content": "You are a coding wizard who explains things with terrible puns and pop culture references."
}
```

### User Prompts
This is where you come in! Your prompts should be:
- Crystal clear
- One thing at a time, you dont wanna confuze it.
- Properly formatted (not really needed but if you want consistent results it is better)

Here's how to ask a question without confusing our silicon friend:
```json
{
  "role": "user",
  "content": "How do I read a file in Python without making it angry?"
}
```

## Code Examples

### Python
```python
import requests
import json

def ask_ai(question, system_prompt=None):
    messages = []
    
    # Tell the AI who it should be today
    if system_prompt:
        messages.append({
            "role": "system",
            "content": system_prompt
        })
    
    # Add your burning question
    messages.append({
        "role": "user",
        "content": question
    })
    
    # Send it off to the AI mothership
    response = requests.post(
        "https://ai.hackclub.com/chat/completions",
        headers={"Content-Type": "application/json"},
        json={"messages": messages}
    )
    
    # Cross fingers and check if it worked
    if response.status_code == 200:
        return response.json()["choices"][0]["message"]["content"]
    else:
        raise Exception(f"Oops! The AI took a coffee break: {response.status_code}")

# Let's take it for a spin!
system_prompt = "You are a coding wizard who explains things with terrible puns."
question = "How do I create a web server in Python?"

try:
    answer = ask_ai(question, system_prompt)
    print(answer)
except Exception as e:
    print(f"Houston, we have a problem: {e}")
```

### Node.js
```javascript
const fetch = require('node-fetch');

async function askAI(question, systemPrompt = null) {
    const messages = [];
    
    // Give the AI its personality for the day
    if (systemPrompt) {
        messages.push({
            role: 'system',
            content: systemPrompt
        });
    }
    
    // Ask away!
    messages.push({
        role: 'user',
        content: question
    });
    
    try {
        const response = await fetch('https://ai.hackclub.com/chat/completions', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ messages })
        });
        
        if (!response.ok) {
            throw new Error(`The AI is having an existential crisis! Status: ${response.status}`);
        }
        
        const data = await response.json();
        return data.choices[0].message.content;
    } catch (error) {
        console.error('Whoopsie!', error);
        throw error;
    }
}

// Time to see if this thing actually works
const systemPrompt = 'You are a coding wizard with a terrible sense of humor.';
const question = 'How do I read a JSON file in Node.js without crying?';

askAI(question, systemPrompt)
    .then(answer => console.log(answer))
    .catch(error => console.error('Well, that didn\'t work:', error));
```

### cURL
```bash
# For those who love the command line
curl -X POST https://ai.hackclub.com/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
        "messages": [
            {
                "role": "system",
                "content": "You are a helpful assistant who makes terrible puns."
            },
            {
                "role": "user",
                "content": "What is the capital of France?"
            }
        ]
    }'
```

guide by minoa.cat <3