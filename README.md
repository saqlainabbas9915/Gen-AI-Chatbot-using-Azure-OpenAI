# Generative AI Chatbot with Azure OpenAI & AI Search

A low-code/no-code chatbot built using **Azure OpenAI Studio** and **Azure AI Search** to interact with custom documents (PDFs). This project demonstrates how to create a RAG (Retrieval-Augmented Generation) application with hybrid search capabilities.

---

## 🚀 Features
- **PDF Interaction**: Chat with your PDFs using semantic search.
- **Low-Code/No-Code**: Built entirely in Azure OpenAI Studio and Azure AI Search.
- **Hybrid Search**: Combines keyword and vector search for accurate responses.
- **Deployment**: Hosted via Azure Blob Storage and integrated with Azure AI services.

---

## 📋 Prerequisites
1. **Azure Account**: Subscription with access to:
   - [Azure OpenAI Service](https://azure.microsoft.com/en-us/products/ai-services/openai-service)
   - [Azure AI Search](https://azure.microsoft.com/en-us/products/ai-services/ai-search)
   - [Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs)
2. **Documents**: PDFs or text files to use as your chatbot's knowledge base.

---

## 🛠️ Setup Guide

### 1. Create Azure Resources
- **Resource Group**:  
  Create a group to organize your services (e.g., `GenAI-Chatbot`).
- **Storage Account**:  
  Set up Blob Storage and create a container (e.g., `documents`) to upload your PDFs.
- **Azure OpenAI Service**:  
  Deploy models in [Azure OpenAI Studio](https://oai.azure.com/):
  - **Text Embedding Model** (e.g., `text-embedding-ada-002`) for vectorization.
  - **Chat Completion Model** (e.g., `GPT-4`) for responses.

### 2. Configure Azure AI Search
1. **Create a Search Service**:  
   Enable hybrid search (vector + keyword) and semantic ranking.
2. **Import Data**:  
   Connect your Blob Storage container to Azure AI Search. The service will:
   - **Chunk documents** into smaller sections (default: 512 tokens).
   - **Generate vectors** using your deployed embedding model.
   - Create a searchable index (e.g., `startup-playbook-index`).

### 3. Integrate with Azure OpenAI Studio
1. **Deploy Models**:  
   - In Azure OpenAI Studio, deploy your text embedding and chat completion models.
   - Note the **API Key**, **Endpoint**, and **Deployment Name**.
2. **Configure Chat Playground**:  
   - Add your Azure AI Search index as a data source.
   - Link the embedding model for prompt vectorization.
   - Enable hybrid search for responses.

### 4. Build the Chat Interface
1. **Use Azure Playground**:  
   Test your chatbot directly in OpenAI Studio’s playground.
2. **Deploy as a Web App**:  
   - Option 1: Use **Power Apps** for a no-code UI.
   - Option 2: Host a static site via **Azure Static Web Apps** ([Example Template](https://github.com/Azure-Samples/azure-openai-samples)).

---

## 🖥️ Usage Example
```python
# Sample API Call to Azure OpenAI
import openai

response = openai.ChatCompletion.create(
    engine="your-chat-deployment",  # e.g., "gpt-4"
    messages=[{"role": "user", "content": "Whats the 5 second rule"}],
    dataSources=[  # Link to Azure AI Search index
        {
            "type": "AzureCognitiveSearch",
            "parameters": {
                "endpoint": "<your-search-endpoint>",
                "key": "<your-search-key>",
                "indexName": "startup-playbook-index"
            }
        }
    ]
)
print(response['choices'][0]['message']['content'])

### 5. 🔧 Troubleshooting
Token Limits: Ensure documents are chunked (default: 512 tokens). Adjust in AI Search settings.

Model Deployment Errors: Verify region compatibility (e.g., East US).