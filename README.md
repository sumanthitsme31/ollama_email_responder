# Local AI Email Auto-Responder

A fully **local, private, and offline-capable** AI-powered email auto-responder built with **n8n** and **Ollama**.

No data leaves your machine. No cloud APIs. Full privacy and control.

---

## ✨ Features

- Fully local AI using Ollama (Llama 3 or any other model)
- Automatic email replies using natural language
- Smart loop prevention (won't reply to itself)
- Maintains proper email threading
- Runs entirely in Docker containers
- Privacy-first — your emails never leave your system
- Easy to customize prompts and behavior

---

## 🛠 Tech Stack

| Component       | Technology          |
|----------------|---------------------|
| Workflow Automation | [n8n](https://n8n.io) |
| Local LLM           | [Ollama](https://ollama.com) |
| Containerization    | Docker + Docker Compose |
| Email Protocols     | IMAP (receive) + SMTP (send) |

---

## Prerequisites

- Docker and Docker Compose installed
- A supported email account (Gmail, Outlook, etc.)
  - For Gmail: Enable IMAP and generate an **App Password**
- Sufficient RAM (8GB+ recommended, 16GB+ ideal for Llama 3 8B)

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/local-ai-email-responder.git
cd local-ai-email-responder


2. Configure Environment Variablesbash

cp .env.example .env

Edit the .env file with your email credentials and preferences.


3. Start the Servicesbash

docker-compose up -d

4. Pull the AI Modelbash

docker exec -it ollama ollama pull llama3:8b

5. Access n8nOpen your browser and go to:
http://localhost:5678Import the workflow.json file (or it may be pre-imported depending on your setup)
Configure IMAP and SMTP credentials in n8n
Activate the workflow

Project Structure

.
├── docker-compose.yml
├── workflow.json
├── .env.example
├── README.md
└── submission.json

ConfigurationEmail Settings (.env)IMAP_HOST, IMAP_USER, IMAP_PASSWORD
SMTP_HOST, SMTP_USER, SMTP_PASSWORD
SMTP_FROM_EMAIL

AI ModelDefault model: llama3:8b
You can change it in the Ollama Generate Reply node in n8n.How It WorksEmail Trigger (IMAP): Polls your inbox for new unread emails
Loop Prevention: Skips emails sent by your own address
Prompt Engineering: Builds a rich prompt with sender, subject, and body
Ollama Inference: Sends prompt to local LLM running in Docker
Smart Reply: Generates polite and concise response
Send Reply (SMTP): Sends reply with proper threading (In-Reply-To)

TestingSend a test email from another account to your monitored email
Wait ~30–90 seconds for processing
Check your inbox for the AI-generated reply
Verify the reply is threaded correctly


Common Issues:n8n can't reach Ollama → Ensure both services are on the same Docker network (ai_responder_net)
Model not found → Run docker exec -it ollama ollama pull llama3:8b
Authentication failed → Use App Password for Gmail
Slow responses → Use a smaller model like phi3 or gemma2:2b

