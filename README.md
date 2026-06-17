# 🤖 AI Chatbot API — Multi-Mode Conversational AI

A production-ready conversational AI backend built with **FastAPI** and **OpenAI GPT-4o-mini**, featuring configurable AI personas, multishot few-shot prompt injection, real-time token tracking, and live cost estimation.

![Python](https://img.shields.io/badge/Python-3.11+-blue?style=flat-square&logo=python)
![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green?style=flat-square&logo=fastapi)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-black?style=flat-square&logo=openai)
![Railway](https://img.shields.io/badge/Deployed-Railway-purple?style=flat-square)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎭 **3 AI Personas** | Gen Z Friend, Teacher, and Storyteller — each with a handcrafted system prompt and injected few-shot examples |
| 🎯 **Multishot Injection** | Few-shot example pairs are injected per mode to prime the model's tone and style before the first user message |
| ⚙️ **Configurable Prompts** | System prompt is a runtime variable controlled via the `mode` API parameter — swap personas without restarting |
| 🔒 **Injection Defence** | All user input wrapped in XML tags to prevent prompt injection and instruction hijacking |
| 📊 **Token Counting** | Input and output tokens tracked on every API call via OpenAI's `usage` response field |
| 💰 **Cost Estimation** | Estimated USD cost returned with every response, calculated from live token counts and model pricing |
| 🧠 **Session Memory** | Per-session conversation history maintained in-memory across turns |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Python 3.11+ |
| API Framework | FastAPI |
| AI Provider | OpenAI GPT-4o-mini |
| Validation | Pydantic v2 |
| Async Client | asyncio / AsyncOpenAI |
| Deployment | Railway |

---

## 🚀 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Health check — returns status and active session IDs |
| `GET` | `/modes` | Lists all available personas with descriptions |
| `POST` | `/chat` | Send a message — returns AI response with token and cost stats |
| `GET` | `/history/{sessionId}` | Returns visible conversation history (excludes internal scaffolding) |
| `DELETE` | `/session/{sessionId}` | Clears session — use when switching modes |

---

## 📡 Request & Response

### `POST /chat`

**Request body:**
```json
{
  "sessionId": "user-abc-123",
  "message": "explain recursion to me",
  "mode": "teacher"
}
```

**Response:**
```json
{
  "session_id": "user-abc-123",
  "mode": "teacher",
  "response": "Great question! Think of recursion like...",
  "history_length": 5,
  "token_stats": {
    "input_tokens": 412,
    "output_tokens": 138,
    "total_tokens": 550,
    "estimated_cost_usd": 0.00014490
  }
}
```

### `GET /modes`

```json
{
  "modes": {
    "genz": "Chill Gen Z best friend — supportive, funny, slang-heavy",
    "teacher": "Patient and clear teacher — explains anything step by step",
    "storyteller": "Vivid co-author — builds immersive stories with you"
  }
}
```

---

## 🎭 Personas

### 🧢 Gen Z Friend
Chill, supportive, and funny. Responds like a close friend over text — short, slang-heavy, emotionally intelligent. Handles sensitive topics with empathy and de-escalates without losing the vibe.

### 📚 Teacher
Patient, clear, and structured. Explains any subject using analogies and real-world examples. Adapts complexity to the user's apparent level. Ends responses with a check-in to offer deeper explanation.

### 📖 Storyteller
Vivid and cinematic. Co-authors immersive stories with the user across any genre. Asks one focused question to unlock creative direction, then writes in rich, atmospheric prose — always ending on a cliffhanger or a choice.

---

## 🧠 Prompt Engineering Approach

Each mode is built on five deliberate layers:

1. **System prompt** — sets identity, tone, knowledge scope, behaviour rules, and edge-case handling for each persona
2. **Few-shot examples** — 2–3 example conversation pairs injected after the system prompt to prime the model's style before the first real message
3. **XML input wrapping** — all user messages wrapped in `<user_input>` tags to prevent prompt injection attacks
4. **Temperature tuning** — default `0.7` to balance creativity and consistency across all modes
5. **Iterative testing** — each mode tested across 10+ conversation types to document performance characteristics

---

## ⚙️ Setup

```bash
# Clone the repo
git clone https://github.com/yourusername/ai-chatbot-api
cd ai-chatbot-api

# Install dependencies
pip install fastapi openai python-dotenv uvicorn

# Configure environment
cp .env.example .env
# Add your OPENAI_API_KEY to .env

# Run locally
uvicorn main:app --reload
```

---

## 🔑 Environment Variables

| Variable | Description |
|---|---|
| `OPENAI_API_KEY` | Your OpenAI API key |

---

## 📁 Project Structure

```
ai-chatbot-api/
├── main.py           # Routes, prompt configs, and business logic
├── .env              # Environment variables (not committed)
├── .env.example      # Template for environment setup
├── requirements.txt  # Python dependencies
└── README.md
```

---

## 📦 Deployment

Deployed on **Railway**. Push to `main` triggers automatic redeploy.

🌐 Live URL: `https://your-app.railway.app`

---

## 📄 License

MIT
