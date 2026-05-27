# 📘 Topic 2 — AI Landscape, Python Fundamentals, REST APIs, FastAPI & LLM Integration
> **Codebasics AI Engineering Bootcamp** · Week 1 of 11  
> Status: 🟡 In Progress

---

## 🗺️ Topic Overview

| Pillar | What You Learn |
|---|---|
| 🌍 AI Landscape | Key LLM providers, frameworks & tools in 2026 |
| 🐍 Python Basics | Variables, functions, classes, lists, dicts |
| 🌐 REST APIs | HTTP methods, requests, JSON payloads |
| ⚡ FastAPI | Build your own AI API server |
| 🤖 LLM Integration | First real Groq API call inside FastAPI |

---

## 🌍 Pillar 1 — The AI Landscape 2026

### Real-World Analogy
> Think of the AI world like the **smartphone industry**.
> You have OS makers (Apple/Google = OpenAI/Anthropic),
> hardware makers (Qualcomm = NVIDIA), and
> app builders (developers = **you**).
> You don't build the OS. You build apps on top of it.

### 🧠 LLM Providers (The Brains)

| Provider | Models | Best For |
|---|---|---|
| **OpenAI** | GPT-4o, o1, o3 | General purpose, most popular |
| **Anthropic** | Claude 3.5, Claude 4 | Reasoning, long context, safety |
| **Google** | Gemini 2.0, 2.5 | Multimodal, search integration |
| **Meta** | Llama 3.3, 4 | Open source, local deployment |
| **Groq** | Fast inference on Llama/Mixtral | Speed — blazing fast responses |
| **Mistral** | Mistral, Mixtral | Lightweight, European, open |

> 💡 For this bootcamp — we use **Groq** (free tier, ultra fast) for most examples.

### 🛠️ Frameworks & Tools

| Category | Tools |
|---|---|
| LLM Orchestration | LangChain, LangGraph, LlamaIndex |
| Vector Databases | Qdrant, Pinecone, Weaviate, FAISS |
| API Servers | FastAPI, Flask |
| Monitoring/Evals | LangSmith, Arize, Helicone |
| Local AI | Ollama, vLLM, llama.cpp |
| Deployment | AWS, GCP, Azure, Railway |

---

## 🐍 Pillar 2 — Python Fundamentals

### Real-World Analogy
> Python is like **plain English for computers**.
> C# is like a formal legal contract — typed, structured, explicit.
> Python is like a sticky note — quick, readable, gets things done.

---

### 2.1 — Variables & Data Types

```python
# Python — dynamic typing
name = "GPT-4"
version = 4.0
is_available = True
max_tokens = 8192
models = ["gpt-4", "claude", "gemini"]
config = {"model": "gpt-4", "temp": 0.7}
```

```csharp
// C# — static typing
string name = "GPT-4";
double version = 4.0;
bool isAvailable = true;
int maxTokens = 8192;
List<string> models = new() { "gpt-4", "claude", "gemini" };
Dictionary<string, object> config = new() { {"model","gpt-4"}, {"temp",0.7} };
```

---

### 2.2 — Functions

```python
# Python function with default parameter
def call_llm(prompt: str, model: str = "llama3-8b-8192") -> str:
    return f"Response from {model} for: {prompt}"

result = call_llm("What is RAG?")
result2 = call_llm("What is RAG?", model="mixtral-8x7b")
```

```csharp
// C# equivalent
string CallLlm(string prompt, string model = "llama3-8b-8192")
{
    return $"Response from {model} for: {prompt}";
}
```

> 🔑 Python uses `def`, C# uses return type before method name. Same concept!

---

### 2.3 — Classes & OOP

```python
# Python Class
class LLMClient:
    def __init__(self, model: str, api_key: str):
        self.model = model
        self.api_key = api_key

    def chat(self, prompt: str) -> str:
        return f"[{self.model}] Response to: {prompt}"

    def __repr__(self):
        return f"LLMClient(model={self.model})"

# Usage
client = LLMClient(model="llama3-8b-8192", api_key="abc123")
print(client.chat("Hello AI!"))
# Output: [llama3-8b-8192] Response to: Hello AI!
```

```csharp
// C# equivalent
public class LLMClient
{
    public string Model { get; set; }
    public string ApiKey { get; set; }

    public LLMClient(string model, string apiKey)
    {
        Model = model;
        ApiKey = apiKey;
    }

    public string Chat(string prompt)
        => $"[{Model}] Response to: {prompt}";

    public override string ToString()
        => $"LLMClient(model={Model})";
}
```

> 🔑 `__init__` = constructor | `self` = `this`

---

### 2.4 — Lists & Dicts (Used EVERYWHERE in AI)

```python
# Building a messages list — core AI pattern!
messages = []
messages.append({"role": "user", "content": "Hello"})
messages.append({"role": "assistant", "content": "Hi there!"})

for msg in messages:
    print(f"{msg['role']}: {msg['content']}")

# List comprehension — Python superpower!
roles = [msg["role"] for msg in messages]
# Output: ["user", "assistant"]
```

```csharp
// C# equivalent
var messages = new List<Dictionary<string, string>>();
messages.Add(new() { ["role"] = "user", ["content"] = "Hello" });
messages.Add(new() { ["role"] = "assistant", ["content"] = "Hi there!" });

// LINQ equivalent of list comprehension
var roles = messages.Select(m => m["role"]).ToList();
```

> 💡 In AI Engineering, **messages lists** and **dicts** are the core data structure in every LLM API call!

---

## 🌐 Pillar 3 — REST APIs

### Real-World Analogy
> A REST API is like a **restaurant menu**.
> You (client) pick an item from the menu (endpoint),
> the waiter (HTTP) carries your order to the kitchen (server),
> which prepares and returns your food (response).

### HTTP Methods

| Method | Action | AI Example |
|---|---|---|
| `GET` | Fetch data | Get list of available models |
| `POST` | Send data, get result | Send prompt → get LLM response |
| `PUT` | Update something | Update a conversation |
| `DELETE` | Remove something | Delete a chat history |

### Calling a REST API in Python

```python
import requests

response = requests.post(
    url="https://api.groq.com/openai/v1/chat/completions",
    headers={
        "Authorization": "Bearer YOUR_API_KEY",
        "Content-Type": "application/json"
    },
    json={
        "model": "llama3-8b-8192",
        "messages": [
            {"role": "user", "content": "What is AI Engineering?"}
        ]
    }
)

data = response.json()
print(data["choices"][0]["message"]["content"])
```

```csharp
// C# equivalent
using var client = new HttpClient();
client.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_API_KEY");

var body = JsonSerializer.Serialize(new {
    model = "llama3-8b-8192",
    messages = new[] {
        new { role = "user", content = "What is AI Engineering?" }
    }
});

var response = await client.PostAsync(
    "https://api.groq.com/openai/v1/chat/completions",
    new StringContent(body, Encoding.UTF8, "application/json")
);
```

---

## ⚡ Pillar 4 — FastAPI

### Real-World Analogy
> FastAPI = **ASP.NET Core Web API — but for Python**.
> Same idea: define routes, handle requests, return responses.
> Bonus: auto-generates Swagger docs at `/docs`!

### Your First FastAPI Server

```python
# main.py
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Request body model (like a C# DTO / record)
class PromptRequest(BaseModel):
    prompt: str
    model: str = "llama3-8b-8192"

@app.get("/")
def root():
    return {"message": "AI API is running!"}

@app.post("/chat")
def chat(request: PromptRequest):
    return {
        "model": request.model,
        "response": f"Echo: {request.prompt}"
    }
```

```bash
# Run the server
uvicorn main:app --reload
# Swagger UI: http://localhost:8000/docs
```

```csharp
// C# ASP.NET Core equivalent
[ApiController]
[Route("[controller]")]
public class ChatController : ControllerBase
{
    [HttpGet("/")]
    public IActionResult Root()
        => Ok(new { message = "AI API is running!" });

    [HttpPost("/chat")]
    public IActionResult Chat([FromBody] PromptRequest request)
        => Ok(new { model = request.Model, response = $"Echo: {request.Prompt}" });
}
```

> 🔑 `@app.post("/chat")` = `[HttpPost("/chat")]`
> `BaseModel` = your DTO/record class
> FastAPI auto-generates Swagger at `/docs` — just like Swashbuckle!

---

## 🤖 Pillar 5 — First Real LLM Integration with Groq

### Full Working App

```python
# full_app.py
import os
from fastapi import FastAPI
from pydantic import BaseModel
from groq import Groq
from dotenv import load_dotenv

load_dotenv()

app = FastAPI(title="My First AI API")
client = Groq(api_key=os.getenv("GROQ_API_KEY"))

class PromptRequest(BaseModel):
    prompt: str
    model: str = "llama3-8b-8192"

@app.get("/")
def root():
    return {"status": "AI API running 🚀"}

@app.post("/chat")
def chat(request: PromptRequest):
    response = client.chat.completions.create(
        model=request.model,
        messages=[
            {"role": "user", "content": request.prompt}
        ]
    )
    return {
        "model": request.model,
        "response": response.choices[0].message.content
    }
```

```bash
# Install & run
pip install groq fastapi uvicorn python-dotenv
uvicorn full_app:app --reload

# Test with curl
curl -X POST "http://localhost:8000/chat" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "What is RAG in AI?"}'
```

> 🎉 You just built your first AI-powered REST API! This is the foundation of EVERY project in this bootcamp.

---

## 🧠 Key Concepts Glossary

| Concept | Simple Definition |
|---|---|
| **Groq** | Ultra-fast LLM inference provider (free tier available) |
| **FastAPI** | Python web framework for building APIs — like ASP.NET Core |
| **Pydantic BaseModel** | Python's way of defining typed request/response schemas (like a DTO) |
| **uvicorn** | The server that runs FastAPI apps (like Kestrel for .NET) |
| **requests** | Python library for making HTTP calls (like HttpClient) |
| **venv** | Virtual environment — isolates Python packages per project |
| **dotenv** | Loads `.env` files into environment variables |
| **List comprehension** | Python shorthand to transform lists in one line |

---

## 📝 Quiz Questions (Topic 2)

1. In FastAPI, what is the equivalent of a C# DTO used to define the shape of a request body?
2. What Python library would you use to call an external REST API?
3. What is the difference between a `GET` and `POST` HTTP method?
4. How do you load API keys safely in Python?
5. What command starts a FastAPI server, and what URL shows the auto-generated docs?

---

## 🚀 Projects Unlocked

| Project | Skills Used | Status |
|---|---|---|
| Market Cap Calculator | Python + OOP | ✅ Ready to build! |
| GitHub Profile Analyzer | External APIs + REST | ✅ Ready to build! |
| Domain-Specific Q&A API | Groq inference + FastAPI | 🔒 After Topic 3 |

---

## 🔗 Resources

- Groq Console: https://console.groq.com
- FastAPI Docs: https://fastapi.tiangolo.com
- Pydantic Docs: https://docs.pydantic.dev
- Python Requests: https://requests.readthedocs.io
- Uvicorn: https://www.uvicorn.org

---

> **Next:** Topic 3 — LLMs Deep Dive, Embeddings & Vector Databases  
> Say `Start Topic 3` when ready!
