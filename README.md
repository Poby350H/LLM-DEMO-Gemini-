Vertex AI Gemini LLM Demo Server

Flask + Google Vertex AI Gemini 2.x + Cloud Run

Live Demo:
https://vertex-ai-flask-754004678844.us-central1.run.app/chat

This project is a lightweight LLM demo server built with Python Flask and deployed on Google Cloud Run.
It provides both a web-based chat interface and a JSON API for running Google Vertex AI Gemini models.
Users can dynamically select which Gemini model to use before submitting a prompt.

---

1. Overview

This server demonstrates how to build a clean, production-like backend around Google’s Gemini models.
Unlike simple notebook experiments, this project includes:

A web UI with model selection

A REST API

A grounding prompt mechanism

A containerized Cloud Run deployment

Service-account–based authentication (no static keys in code)

It is designed for portfolio use as well as practical LLM backend demonstrations.

---

2. Key Features

Supports multiple Gemini models (selectable at runtime)

Provides a full web chat interface (/chat)

Provides a JSON API interface (/ask)

Injects a grounding prompt for consistent and factual answers

Fully containerized and deployed on Cloud Run

Uses Application Default Credentials (ADC) — no embedded service keys

Lightweight, fast, and suitable for public demo sharing

---

3. Supported Models

The following Gemini models are available via the UI dropdown or via the API:

Model ID	Description
gemini-2.0-flash	Gemini 2.0 Flash — legacy, stable
gemini-2.5-flash	Gemini 2.5 Flash — balanced & versatile (default)
gemini-2.5-flash-lite	Gemini 2.5 Flash Lite — fastest & cheapest
gemini-2.5-pro	Gemini 2.5 Pro — best reasoning, higher cost

Model selection is passed directly to Vertex AI during generation.

---

4. Grounding Prompt

A key component of the server is the grounding prompt, which wraps the user’s input with contextual and safety instructions.

Example (internally generated):

The current time is 2025-11-29 15:40.
Using this information as context, please answer the user's question as an expert.
Avoid speculation and base your answer only on factual information.

User question: {USER_PROMPT}


Purpose:

Ensures consistent tone

Reduces hallucination

Helps the model understand current context

Works especially well with lightweight models such as Flash or Flash-Lite

---
5. Endpoints
5.1 Web UI

GET /chat

Model selector

Text prompt input

Displays formatted response

Useful for demo and manual testing

5.2 JSON API

POST /ask

```json

Example request:

{
  "prompt": "Explain vector embeddings.",
  "model": "gemini-2.5-pro"
}
{
  "prompt": "Explain vector embeddings.",
  "model": "gemini-2.5-pro"
}



Example response:

{
  "model": "gemini-2.5-pro",
  "prompt": "Explain vector embeddings.",
  "answer": "..."
}
```
---

6. Project Structure

```
Vertex_AI/
 ├── app.py                # Flask app + Gemini integration
 ├── Dockerfile            # Cloud Run container definition
 ├── requirements.txt      # Python dependencies
 └── templates/
       └── chat.html       # UI template (model selector + chat)
```
---

7. Core Logic Example
def ask_gemini(prompt, model_name=None):
    if not model_name:
        model_name = DEFAULT_MODEL

    aiplatform.init(project=PROJECT_ID, location=LOCATION)
    model = GenerativeModel(model_name)

    response = model.generate_content(
        prompt,
        generation_config={"max_output_tokens": 2048, "temperature": 0.4}
    )
    return response.text

---
8. Tech Stack
Area	Technology
Backend	Python Flask
AI Model	Google Vertex AI (Gemini 2.x family)
Deployment	Cloud Run (serverless), Cloud Build
Authentication	Service Account (ADC)
Frontend	Jinja2 template
---
10. Portfolio Relevance

This project demonstrates:

Ability to integrate Vertex AI generative models

Ability to build a clean API and UI around LLM services

Practical cloud deployment using Cloud Run and Docker

Proper prompt-engineering practices (grounding prompt)

Real-world backend design separating UI, API, and model logic

---
