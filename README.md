# 📧 Customer Support Email Workflow (n8n)

An automated customer support pipeline built with **n8n** that monitors a Gmail inbox, classifies incoming emails using AI, and automatically replies to customer queries using a RAG-powered AI agent backed by a Pinecone knowledge base.

---

## 🔄 Workflow Overview
```
Gmail Trigger → Text Classifier → [Customer Support] → AI Agent → Reply to Message
                                → [Others]          → No Operation
```

### Nodes

| Node | Purpose |
|------|---------|
| **Gmail Trigger** | Polls inbox every minute for new emails |
| **Text Classifier** | Uses GPT-4o-mini to classify emails as `Customer Support` or `Others` |
| **AI Agent** | GPT-4 powered agent that drafts friendly, on-brand replies |
| **Pinecone Vector Store** | Retrieves relevant FAQs and policy docs via RAG |
| **Embeddings OpenAI** | Generates 512-dimension embeddings for semantic search |
| **Reply to a Message** | Sends the AI-generated reply back to the original email thread |
| **No Operation** | Silently drops non-support emails |

---

## 🧠 How It Works

1. **Gmail Trigger** polls the inbox every minute for unread emails.
2. **Text Classifier** (powered by `gpt-4o-mini`) categorizes each email:
   - `Customer Support` → proceeds to AI Agent
   - `Others` → dropped silently
3. **AI Agent** (powered by `gpt-4`) reads the email and queries the **Pinecone Vector Store** for relevant policy/FAQ information.
4. The agent composes a friendly, emoji-enhanced reply signed as *Mr. Helpful from Tech Haven Solutions*.
5. **Gmail node** sends the reply in the same email thread.

---

## 🛠️ Tech Stack

- [n8n](https://n8n.io/) — workflow automation
- [OpenAI GPT-4 / GPT-4o-mini](https://openai.com/) — language models
- [Pinecone](https://www.pinecone.io/) — vector database for RAG
- Gmail OAuth2 — email trigger and reply

---

## ⚙️ Setup Instructions

### 1. Prerequisites

- A running n8n instance
- OpenAI API key
- Pinecone account with an index named `ragchatbot` (namespace: `FAQs`)
- Gmail account with OAuth2 credentials configured in n8n

### 2. Import the Workflow

1. Download `customer_support_workflow.json`
2. In n8n, go to **Workflows → Import from File**
3. Select the JSON file

### 3. Configure Credentials

| Credential | Used By |
|------------|---------|
| Gmail OAuth2 | Gmail Trigger, Reply to a Message |
| OpenAI API | Text Classifier, AI Agent, Embeddings OpenAI |
| Pinecone API | Pinecone Vector Store |

### 4. Populate Your Knowledge Base

Upload your FAQs and policy documents to Pinecone:
- **Index:** `ragchatbot`
- **Namespace:** `FAQs`
- **Embedding dimensions:** `512`

### 5. Activate the Workflow

Toggle the workflow to **Active** in n8n.

---

## 🤖 AI Agent Persona

The agent is prompted to act as a customer support representative for **Tech Haven**:
- Responds in a friendly tone with emojis 😊
- Signs off as **Mr. Helpful from Tech Haven Solutions**
- Outputs only the email body (no subject line or headers)

---

## 📌 Notes

- Non-customer-support emails are silently ignored via the **No Operation** node
- The workflow uses `gpt-4o-mini` for fast classification and `gpt-4` for high-quality response generation
- Pinecone embeddings use 512 dimensions for efficiency

---
