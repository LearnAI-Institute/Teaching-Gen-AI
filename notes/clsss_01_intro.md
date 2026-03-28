# Class 1: Introduction to Generative AI

This session introduces the core concepts behind Generative AI — what it is, how it works, and why it matters. Before writing a single line of code, it is important to build a solid mental model of the technology you are working with.

---

## 1. What is Generative AI?

**Generative AI** refers to AI systems that can *create new content* — text, images, audio, code, and more — rather than simply classifying or analyzing existing data.

| Traditional AI | Generative AI |
| :--- | :--- |
| Classifies emails as spam/not spam | Writes a reply to an email |
| Detects a cat in a photo | Draws a new picture of a cat |
| Predicts house prices | Writes a property listing |
| Translates a sentence | Holds a full conversation |

The key shift is from **"recognize and decide"** to **"understand and create."**

---

## 2. What is a Large Language Model (LLM)?

A **Large Language Model (LLM)** is the engine behind most modern Generative AI applications. It is a neural network trained on hundreds of billions of words from books, websites, code, and scientific papers.

### How does it "think"?
The model does not actually "think." It predicts the **most likely next word (token)** based on everything it has seen so far. Repeat this process thousands of times and you get a full paragraph, essay, or code file.

```
Input Prompt:  "The capital of France is"
LLM Predicts:  "Paris"
Next token:    "which"
Next token:    "is"
Next token:    "known"
... and so on.
```

### Key Parameters You Will Encounter:

| Parameter | What It Controls | Example |
| :--- | :--- | :--- |
| `model` | Which AI model to use | `"gpt-4o-mini"`, `"gemini-1.5-flash"` |
| `temperature` | Creativity / randomness (0.0 = focused, 1.0 = creative) | `0.7` |
| `max_tokens` | Maximum length of the response | `512` |

---

## 3. What are Tokens?

LLMs do not read word by word — they read in **tokens**. A token is roughly 3–4 characters, or about 0.75 words.

```
Sentence:  "I love programming with LangChain!"
Tokens:    ["I", " love", " programming", " with", " Lang", "Chain", "!"]
Count:     7 tokens
```

**Why does this matter?**
- Every API call costs money based on token count (input + output).
- Every model has a **context window** — a maximum number of tokens it can process at once (e.g., GPT-4o has a 128,000 token context window).
- Keeping prompts efficient saves cost and improves speed.

---

## 4. Project Directory Structure

A clean structure separates the learning phase from the production phase. Use this layout throughout the course:

```
your-project/
│
├── notebooks/          # Jupyter notebooks for experimentation
├── apps/               # Final, complete applications
├── requirements.txt    # All Python dependencies listed here
├── .env                # Secret API keys (NEVER commit this to GitHub)
└── .gitignore          # Tells Git to ignore .env and venv/
```

### What each folder does:

* **`notebooks/`** — Run code line-by-line, test prompts, observe model outputs step by step.
* **`apps/`** — Store finished projects like Streamlit chatbots or FastAPI services.
* **`requirements.txt`** — Lists every library your project needs. Anyone can reproduce your setup with `pip install -r requirements.txt`.
* **`.env`** — Stores API keys as environment variables so they are never hardcoded into source code.

---

## 5. Environment Setup (Virtual Environment)

A **Virtual Environment** isolates your project's dependencies so they do not conflict with other Python projects on your machine.

### Step-by-Step Setup:

**Step 1: Create the virtual environment**
```bash
python -m venv venv
```

**Step 2: Activate it**
```bash
# Windows
.\venv\Scripts\activate

# Mac / Linux
source venv/bin/activate
```

After activation, your terminal prompt will show `(venv)` at the start — this means you are inside the isolated environment.

**Step 3: Install dependencies**
```bash
pip install langchain langchain-openai langchain-google-genai python-dotenv
```

**Step 4: Save your dependencies**
```bash
pip freeze > requirements.txt
```

---

## 6. API Key Setup & Security

Generative AI development connects to powerful cloud models via **APIs**. Each provider gives you a secret key to authenticate your requests.

### The 5 Providers in This Course:

| Provider | Purpose | Where to Get Key |
| :--- | :--- | :--- |
| **OpenAI** | GPT-4o, GPT-4o-mini models | platform.openai.com |
| **Google Gemini** | Gemini 1.5 Pro / Flash models | aistudio.google.com |
| **Anthropic** | Claude 3.5 Sonnet / Haiku models | console.anthropic.com |
| **Groq** | Ultra-fast inference using LPUs | console.groq.com |
| **HuggingFace** | Open-source models (Llama, Mistral) | huggingface.co/settings/tokens |

### Storing Keys Safely in `.env`:
```env
OPENAI_API_KEY="sk-proj-..."
GOOGLE_API_KEY="AIza..."
ANTHROPIC_API_KEY="sk-ant-..."
GROQ_API_KEY="gsk_..."
HUGGINGFACE_API_KEY="hf_..."
```

### Loading Keys in Python:
```python
import os
from dotenv import load_dotenv

# Reads the .env file and loads all variables into the environment
load_dotenv()

# Access a key by name — returns None if the key is missing
openai_key = os.getenv("OPENAI_API_KEY")
print("Key loaded:", openai_key[:10], "...")  # Print only the first 10 chars for safety
```

> **Security Rule:** Add `.env` to your `.gitignore` file immediately. Never upload API keys to GitHub — even accidentally. Leaked keys result in unauthorized charges to your account.

---

## 7. Your First API Test

Once your environment is set up and your `.env` file is ready, run this simple test to confirm everything is connected:

```python
import os
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI

# Load API keys
load_dotenv()

# Initialize the model
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0.7)

# Send a simple message
response = llm.invoke("What is Generative AI in one sentence?")

# Print the response text
print(response.content)
```

**Expected Output:**
```
Generative AI is a type of artificial intelligence that creates new content
such as text, images, or code by learning patterns from large datasets.
```

If you see a response like this — your setup is working correctly!

---

## 8. Credits and Billing

While some tiers offer free access, production-grade models require a small initial investment:

* **OpenAI**: Load a minimum of **$5** to enable API access.
* **Anthropic**: Load a minimum of **$5** to enable API access.
* **Groq & HuggingFace**: Have generous free tiers for learning.
* **Google Gemini**: Offers a free tier sufficient for this course.

The ~$10 investment (roughly ₹900) covers the entire learning series and is a necessary step to build real-world applications.

---

## 9. Summary Checklist for Class 1
* [ ] Understand what Generative AI and LLMs are.
* [ ] Understand what tokens are and why they matter.
* [ ] Set up the project folder structure.
* [ ] Create and activate a Python virtual environment.
* [ ] Create the `.env` file and add API keys.
* [ ] Add `.env` to `.gitignore`.
* [ ] Run a successful first API test.

---

### 💡 Analogy: The Professional Kitchen

Setting up your Gen AI environment is like **organizing a professional kitchen**:

* **The `.env` file** is your locked pantry — it holds expensive, secret ingredients (API keys) that only you should access.
* **The `notebooks/` folder** is your test kitchen — you experiment with recipes here before committing to them.
* **The `apps/` folder** is the dining room — this is where the finished meals are served to guests.
* **The Virtual Environment** is a clean, isolated workstation — ingredients from one dish never contaminate another.
* **`requirements.txt`** is your recipe card — anyone can replicate your exact kitchen setup from this file.
