# Class 2: Project Architecture & API Security

The objective of this session is to establish a **professional workspace** for Generative AI development. We focus on creating a scalable directory structure, implementing robust API security, and verifying connections to multiple LLM providers.

---

## 1. Project Directory Structure

In Gen AI development, we distinguish between two phases: **Experimentation** and **Deployment**. A clean, standardized structure keeps both phases organized and separated.

```
your-project/
│
├── notebooks/              # Jupyter Notebooks for learning and prototyping
│   ├── 01_tokenization.ipynb
│   ├── 02_first_chat.ipynb
│   └── 03_prompt_templates.ipynb
│
├── apps/                   # Production-ready applications
│   ├── translator_app.py
│   └── chatbot_app.py
│
├── requirements.txt        # All Python dependencies
├── .env                    # Secret API keys (never commit this!)
└── .gitignore              # Files Git should ignore
```

### Why This Separation Matters:

| Phase | Location | Purpose |
| :--- | :--- | :--- |
| Experimentation | `notebooks/` | Test ideas, observe outputs, learn step-by-step |
| Deployment | `apps/` | Clean, complete scripts ready for users |

---

## 2. Virtual Environments (Venv) — Deep Dive

A **Virtual Environment** is an isolated Python installation for your project. It prevents **"dependency hell"** — where one project needs `library v1.0` and another needs `library v2.0`, and they conflict on the same machine.

### Visual Explanation:
```
Without Venv:          With Venv:
─────────────          ──────────────────────────────
  System Python          System Python
  └── langchain 0.1       ├── Project A (venv)
  └── openai 0.28         │    ├── langchain 0.3
  (conflicts!)            │    └── openai 1.0
                          └── Project B (venv)
                               ├── langchain 0.1
                               └── openai 0.28
```

### Complete Setup Workflow:

**Step 1 — Create the environment:**
```bash
python -m venv venv
```
This creates a `venv/` folder in your project directory containing a fresh Python installation.

**Step 2 — Activate the environment:**
```bash
# Windows (Command Prompt / PowerShell)
.\venv\Scripts\activate

# Mac / Linux (Bash / Zsh)
source venv/bin/activate
```

Your terminal prompt will now show `(venv)` — confirming you are inside the isolated environment.

**Step 3 — Install required libraries:**
```bash
pip install langchain langchain-openai langchain-google-genai langchain-anthropic langchain-groq python-dotenv
```

**Step 4 — Save your dependencies:**
```bash
pip freeze > requirements.txt
```

**Step 5 — Later, restore on a new machine:**
```bash
pip install -r requirements.txt
```

---

## 3. Environment Variables & API Security

### The Problem with Hardcoded Keys

This is the **wrong** way to use API keys:
```python
# ❌ NEVER DO THIS — if pushed to GitHub, your key is exposed!
import openai
openai.api_key = "sk-proj-abc123xyz..."
```

If this code is pushed to a public GitHub repository, bots scan for API keys within seconds. You could receive unexpected charges or have your account suspended.

### The Correct Approach — `.env` + `python-dotenv`

**Step 1 — Create the `.env` file** in your project root:
```env
# .env — Store all your secret keys here
OPENAI_API_KEY="sk-proj-..."
GOOGLE_API_KEY="AIza..."
ANTHROPIC_API_KEY="sk-ant-..."
GROQ_API_KEY="gsk_..."
HUGGINGFACE_API_KEY="hf_..."
```

**Step 2 — Create `.gitignore`** to prevent `.env` from being committed:
```text
# .gitignore
.env
venv/
__pycache__/
*.pyc
.DS_Store
```

**Step 3 — Load keys in Python using `python-dotenv`:**
```python
import os
from dotenv import load_dotenv

# Reads .env and loads all key=value pairs into the environment
load_dotenv()

# Safely retrieve each key
openai_key   = os.getenv("OPENAI_API_KEY")
google_key   = os.getenv("GOOGLE_API_KEY")
anthropic_key = os.getenv("ANTHROPIC_API_KEY")
groq_key     = os.getenv("GROQ_API_KEY")

# Verify keys loaded (print only first 10 characters for safety)
print(f"OpenAI Key   : {openai_key[:10]}...")
print(f"Google Key   : {google_key[:10]}...")
print(f"Anthropic Key: {anthropic_key[:10]}...")
print(f"Groq Key     : {groq_key[:10]}...")
```

---

## 4. Connecting to Multiple LLM Providers

One of LangChain's biggest strengths is that you can connect to **any** major LLM provider with a nearly identical interface. Here is how to initialize each one:

### OpenAI (GPT-4o-mini)
```python
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv

load_dotenv()

llm_openai = ChatOpenAI(model="gpt-4o-mini", temperature=0.7)
response = llm_openai.invoke("What is a neural network?")
print("OpenAI:", response.content)
```

### Google Gemini (Gemini 1.5 Flash)
```python
from langchain_google_genai import ChatGoogleGenerativeAI
from dotenv import load_dotenv

load_dotenv()

llm_gemini = ChatGoogleGenerativeAI(model="gemini-1.5-flash", temperature=0.7)
response = llm_gemini.invoke("What is a neural network?")
print("Gemini:", response.content)
```

### Anthropic Claude (Claude 3.5 Haiku)
```python
from langchain_anthropic import ChatAnthropic
from dotenv import load_dotenv

load_dotenv()

llm_claude = ChatAnthropic(model="claude-haiku-4-5-20251001", temperature=0.7)
response = llm_claude.invoke("What is a neural network?")
print("Claude:", response.content)
```

### Groq (Llama 3 — Ultra Fast)
```python
from langchain_groq import ChatGroq
from dotenv import load_dotenv

load_dotenv()

llm_groq = ChatGroq(model="llama3-8b-8192", temperature=0.7)
response = llm_groq.invoke("What is a neural network?")
print("Groq:", response.content)
```

> Notice how `.invoke("question")` works **identically** across all providers. This is the power of LangChain's abstraction layer.

---

## 5. The `requirements.txt` File

This file is a **contract** — it tells anyone who clones your project exactly which libraries and versions to install.

### Example `requirements.txt` for this course:
```text
langchain>=0.3.0
langchain-openai>=0.2.0
langchain-google-genai>=2.0.0
langchain-anthropic>=0.2.0
langchain-groq>=0.2.0
python-dotenv>=1.0.0
jupyter>=1.0.0
notebook>=7.0.0
```

### Install all at once:
```bash
pip install -r requirements.txt
```

---

## 6. Verifying Your Full Setup

Run this checklist script to verify that all providers are connected and responding:

```python
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain_google_genai import ChatGoogleGenerativeAI

load_dotenv()

test_question = "Say 'Setup successful!' and nothing else."

# --- Test 1: OpenAI ---
try:
    llm = ChatOpenAI(model="gpt-4o-mini")
    res = llm.invoke(test_question)
    print(f"✅ OpenAI Connected   : {res.content.strip()}")
except Exception as e:
    print(f"❌ OpenAI Failed      : {e}")

# --- Test 2: Google Gemini ---
try:
    llm = ChatGoogleGenerativeAI(model="gemini-1.5-flash")
    res = llm.invoke(test_question)
    print(f"✅ Gemini Connected   : {res.content.strip()}")
except Exception as e:
    print(f"❌ Gemini Failed      : {e}")

print("\nSetup verification complete.")
```

**Expected Output:**
```
✅ OpenAI Connected   : Setup successful!
✅ Gemini Connected   : Setup successful!

Setup verification complete.
```

---

## 7. Summary Checklist for Class 2
* [ ] Understand the `notebooks/` vs `apps/` folder separation.
* [ ] Create and activate a virtual environment.
* [ ] Install all required LangChain packages.
* [ ] Create `.env` file with all API keys.
* [ ] Add `.env` to `.gitignore`.
* [ ] Load keys securely using `python-dotenv`.
* [ ] Successfully call at least one LLM provider using LangChain.
* [ ] Run the setup verification script.

---

### 💡 Analogy: The Security Badge System

Think of API keys as **security badges** for an office building:

* **The `.env` file** is your badge wallet — it holds all your access cards in one secure place.
* **`python-dotenv`** is the security scanner — it reads your badge and grants you access without you shouting your badge number across the room.
* **`.gitignore`** is the rule that says "leave your badge wallet at home before posting your work on the public notice board."
* **`os.getenv()`** is the front desk receptionist — you ask for access, they verify your badge, and let you through.
