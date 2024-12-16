---
title: 'AI Agents with Hugging Face'
layout: '../../layouts/Docs.astro'
---

# How it Works

The project is built around **Hugging Face** integrations for AI models. It provides a modular and scalable approach to creating, deploying, and customizing AI agents. By leveraging Hugging Face's extensive API ecosystem and model hub, developers can seamlessly add new features or AI capabilities to the platform.

The core structure consists of:
- **Model APIs**: Leveraging Hugging Face models (NLP, vision, and more) via APIs.
- **Custom Components**: Adding new features and agents with minimal configuration.
- **Community Contributions**: A simple process to add and share new AI features.

---

## Core Structure

### 1. API Integration

The project uses Hugging Face's API endpoints to integrate pre-trained AI models. These models can range from text generation, summarization, translation, image recognition, and more.

To integrate any model, you can use the Hugging Face Inference API:

```javascript
// app/api/chat/route.ts
import { NextResponse } from 'next/server'
import { HfInference } from "@huggingface/inference"

const MODEL = "Qwen/Qwen2.5-Coder-32B-Instruct"

**might wanna add a specilization feature on your ai **

export async function POST(request: Request) {
  try {
    const { message, address } = await request.json()

    if (!message) {
      return NextResponse.json({ error: "Missing 'message' field in the request body" }, { status: 400 })
    }

    if (!process.env.HUGGING_FACE_TOKEN) {
      return NextResponse.json({ error: "Missing Hugging Face token" }, { status: 500 })
    }

    console.log('Sending request to Hugging Face API:', {
      model: MODEL,
      message: message,
      address: address,
      token: process.env.HUGGING_FACE_TOKEN ? 'Present' : 'Missing'
    })

    const client = new HfInference(process.env.HUGGING_FACE_TOKEN)

    const chatCompletion = await client.chatCompletion({
      model: MODEL,
      messages: [
        {
          role: "system",
          content: systemPrompt
        },
        {
          role: "user",
          content: `User address: ${address}\nUser message: ${message}`
        }
      ],
      max_tokens: 500
    })

    console.log('Received response from Hugging Face API:', chatCompletion)

    return NextResponse.json(chatCompletion.choices[0].message, {
      headers: {
        "Content-Type": "application/json"
      }
    })
  } catch (error: any) {
    console.error('Error in chat API:', error)
    return NextResponse.json({ error: "Internal server error", details: error.message }, { status: 500 })
  }
}
```

**Note**: Replace `your-api-key` with your Hugging Face API token.

### 2. Adding New Features

To add a new AI-powered feature, follow these steps:

1. **Identify a Model**: Search for relevant models on the [Hugging Face Model Hub](https://huggingface.co/models).
2. **Integrate the API**: Use the Hugging Face Inference API or libraries (Transformers, Diffusers) to connect to the model.
3. **Build a Component**: Create a modular component for your feature.
   - Example: If you want to add a summarization tool, wrap the API logic into a clean, reusable function or component.
4. **Register the Feature**: Add your new feature to the `features` directory for easy access.

#### Example: Adding a Summarization Feature

```javascript
import { useState } from 'react'
 
export interface Message {
  id: string
  role: 'user' | 'assistant'
  content: string
}

interface UseChatProps {
  initialMessages: Message[]
}

export function useChat({ initialMessages }: UseChatProps) {
  const [messages, setMessages] = useState<Message[]>(initialMessages)
  const [input, setInput] = useState('')
  const [isLoading, setIsLoading] = useState(false)
  const [error, setError] = useState<string | null>(null)
  const { publicKey } = useWallet()

  return { messages, input, handleInputChange, handleSubmit, isLoading, error }
}

```

This function can now be reused across the project to summarize text inputs.

---

## Contributing Features

The project is designed to encourage community contributions. If you want to add a new AI agent or feature, follow this workflow:

### Steps to Contribute:

1. **Fork the Repository**: Clone the project and set up your local environment.
2. **Add Your Feature**:
   - Create a new folder/file under `src/features/`.
   - Integrate the Hugging Face model of your choice.
   - Wrap the logic into a reusable function/component.
3. **Test the Feature**: Ensure that your feature works correctly and efficiently.
4. **Submit a Pull Request**: Open a PR describing your feature, its use case, and the Hugging Face model used.

---

## Examples of Features

Here are some AI-powered features that can be easily added:

### Text Generation
- **Model**: `EleutherAI/gpt-neo-2.7B`
- **Use Case**: Generate creative text, responses, or content based on a given prompt.

### Text Summarization
- **Model**: `facebook/bart-large-cnn`
- **Use Case**: Summarize long-form content into concise versions.

### Sentiment Analysis
- **Model**: `distilbert-base-uncased-finetuned-sst-2-english`
- **Use Case**: Analyze text sentiment (positive/negative/neutral).

### Image Recognition
- **Model**: `google/vit-base-patch16-224`
- **Use Case**: Classify or describe images using AI.

### Image Generation
- **Model**: `stabilityai/stable-diffusion-2`
- **Use Case**: Generate images from text prompts.

---

## Project Structure

### Key Folders

- **`/src/features`**: All AI feature components.
- **`/public`**: Static assets.
- **`/docs`**: Documentation for contributors and users.

### Adding Models

New Hugging Face models can be added dynamically using the Hugging Face API or by locally hosting models with libraries like Transformers or Diffusers.

### API Key Management

Keep your Hugging Face API token secure by storing it in environment variables:

```bash
# .env file
HF_TOKEN=your-api-key
```

Access it in your project:

```javascript
const apiKey = process.env.HF_TOKEN;
```

---

## Conclusion

By leveraging Hugging Face's ecosystem, this project enables quick and seamless integration of AI features. Whether you're adding NLP capabilities, vision models, or custom workflows, the modular structure makes it simple to expand the platform.

Start building, contribute, and empower others to use AI in innovative ways!
