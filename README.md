# Local AI Email Auto-Responder

A fully **local, private, and offline-capable** AI-powered email auto-responder built with **n8n** and **Ollama**.

No data leaves your machine. No cloud APIs. Full privacy and control.

---

## Features

* Fully local AI using Ollama (Llama 3 or any compatible model)
* Automatic email replies using natural language
* Smart loop prevention (won't reply to itself)
* Maintains proper email threading
* Runs entirely in Docker containers
* Privacy-first — your emails never leave your system
* Easy to customize prompts and behavior

---

## Tech Stack

| Component           | Technology                   |
| ------------------- | ---------------------------- |
| Workflow Automation | n8n                          |
| Local LLM           | Ollama                       |
| Containerization    | Docker & Docker Compose      |
| Email Protocols     | IMAP (receive) & SMTP (send) |

---

## Prerequisites

* Docker installed
* Docker Compose installed
* Email account with IMAP and SMTP enabled
* For Gmail:

  * Enable IMAP
  * Generate an App Password
* Recommended:

  * 8 GB RAM minimum
  * 16 GB RAM preferred for llama3:8b

---

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/local-ai-email-responder.git
cd local-ai-email-responder
```

### 2. Configure Environment Variables

```bash
cp .env.example .env
```

Edit the `.env` file with your email credentials and configuration.

### 3. Start the Services

```bash
docker-compose up -d
```

### 4. Pull the AI Model

```bash
docker exec -it ollama ollama pull llama3:8b
```

### 5. Access n8n

Open:

```text
http://localhost:5678
```

Then:

1. Import `workflow.json`
2. Configure IMAP credentials
3. Configure SMTP credentials
4. Activate the workflow

---

## Project Structure

```text
.
├── docker-compose.yml
├── workflow.json
├── .env.example
├── README.md
└── submission.json
```

---

## Configuration

### Email Settings (.env)

```env
IMAP_HOST=
IMAP_PORT=
IMAP_USER=
IMAP_PASSWORD=

SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASSWORD=

SMTP_FROM_EMAIL=
```

### AI Model

Default model:

```text
llama3:8b
```

You may change the model inside the **Ollama Generate Reply** node within n8n.

Examples:

```text
phi3
mistral
gemma2:2b
llama3:8b
```

---

## How It Works

1. **Email Trigger (IMAP)**

   * Watches the inbox for unread emails.

2. **Loop Prevention**

   * Prevents the workflow from responding to its own emails.

3. **Prompt Construction**

   * Builds a prompt using:

     * Sender
     * Subject
     * Email body

4. **Ollama Inference**

   * Sends the prompt to the local Ollama API.

5. **AI Reply Generation**

   * The LLM creates a concise response.

6. **SMTP Email Reply**

   * Sends the generated response back to the sender.

7. **Thread Preservation**

   * Uses the original Message-ID to keep replies within the same email thread.

---

## Testing

1. Start all containers:

```bash
docker-compose up -d
```

2. Send a test email from another account.

3. Open the n8n dashboard and check workflow executions.

4. Verify:

   * Workflow executes successfully
   * AI-generated reply is sent
   * Reply appears in the same email thread

---

## Troubleshooting

### n8n Cannot Reach Ollama

Ensure both services are attached to the same Docker network:

```text
ai_responder_net
```

### Model Not Found

Pull the model:

```bash
docker exec -it ollama ollama pull llama3:8b
```

### Gmail Authentication Failed

Use a Gmail App Password instead of your regular password.

### Slow Responses

Use a smaller model:

```text
phi3
gemma2:2b
```

instead of:

```text
llama3:8b
```

---

