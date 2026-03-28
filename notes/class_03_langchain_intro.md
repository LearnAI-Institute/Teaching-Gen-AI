# Class 3: Introduction to LangChain & Your First QnA Chatbot

In this session, we dive into **LangChain** — the industry-standard framework for building applications powered by Large Language Models. We move from raw, fragmented API calls to a unified, scalable architecture that works across every major AI provider.

---

## 1. The Core Problem: Provider Fragmentation

Before LangChain, if a developer wanted to switch from OpenAI to Google Gemini, they had to rewrite almost **50% of their code**. Here is why:

### Without LangChain — Raw API Calls:
```python
# --- Using OpenAI directly ---
from openai import OpenAI
client = OpenAI(api_key="sk-...")
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "What is AI?"}]
)
print(response.choices[0].message.content)  # Nested, inconsistent structure


# --- Switching to Google Gemini directly ---
import google.generativeai as genai
genai.configure(api_key="AIza...")
model = genai.GenerativeModel("gemini-1.5-flash")
response = model.generate_content("What is AI?")
print(response.text)  # Completely different method and structure!
```

Notice the problems:
- **Different SDKs**: `openai` library vs `google-generativeai` library.
- **Different methods**: `.create()` vs `.generate_content()`.
- **Inconsistent outputs**: `response.choices[0].message.content` vs `response.text`.

---

## 2. LangChain as the Universal Adapter

**LangChain's Solution:** It acts as an **abstraction layer** — you write your code once using LangChain's standard interface, and switching providers requires changing only **one line**.

```python
# --- With LangChain: OpenAI ---
from langchain_openai import ChatOpenAI
llm = ChatOpenAI(model="gpt-4o-mini")
response = llm.invoke("What is AI?")
print(response.content)  # Always .content, regardless of provider

# --- Switching to Gemini: Only ONE line changes! ---
from langchain_google_genai import ChatGoogleGenerativeAI
llm = ChatGoogleGenerativeAI(model="gemini-1.5-flash")
response = llm.invoke("What is AI?")
print(response.content)  # Same .content — consistent interface!
```

The `.invoke()` method and the `.content` attribute on the response work identically across ALL providers in LangChain.

---

## 3. The LangChain Ecosystem

LangChain is not a single library — it is a **modular stack** of tools designed to work together:

```
┌─────────────────────────────────────────────────────┐
│                   LangChain Stack                    │
├──────────────┬──────────────┬────────────┬──────────┤
│  LangChain   │  LangGraph   │ LangSmith  │LangServe │
│   (Core)     │  (Agents)    │(Monitoring)│  (APIs)  │
├──────────────┴──────────────┴────────────┴──────────┤
│          Your Application Logic (Python)             │
└─────────────────────────────────────────────────────┘
```

| Tool | Purpose | When to Use |
| :--- | :--- | :--- |
| **LangChain Core** | Base library — prompts, models, chains | Every project |
| **LangGraph** | Multi-step, looping AI agents | Complex reasoning tasks |
| **LangSmith** | Debug, test, and monitor AI apps | Development and production |
| **LangServe** | Deploy chains as REST APIs | When building an API backend |

---

## 4. The 5 Pillars of LangChain

LangChain is built around five key components. Think of them as Lego bricks — you mix and match them to build any AI application.

```
┌───────────┐  ┌───────────┐  ┌──────────┐  ┌────────┐  ┌────────┐
│ Model I/O │  │ Retrieval │  │  Memory  │  │ Chains │  │ Agents │
│           │  │  (RAG)    │  │          │  │        │  │        │
│ Prompts + │  │ PDFs,DBs, │  │ Remember │  │ Multi- │  │ Use    │
│ Models +  │  │ Websites  │  │ context  │  │  step  │  │ tools  │
│ Parsers   │  │ as context│  │          │  │  flows │  │        │
└───────────┘  └───────────┘  └──────────┘  └────────┘  └────────┘
   Class 3        Class 6       Class 5       Class 4     Class 7
```

In **Class 3**, we focus on **Model I/O** — the foundation of everything.

---

## 5. Understanding the Message Types

When you communicate with a chat model, LangChain organizes messages into **roles**. This mirrors how a real conversation works:

| Role | Class | Description |
| :--- | :--- | :--- |
| `SystemMessage` | Sets the AI's persona/rules | "You are a helpful coding assistant." |
| `HumanMessage` | The user's input | "How do I reverse a list in Python?" |
| `AIMessage` | The model's response | "You can use `my_list[::-1]`..." |

```python
from langchain_core.messages import SystemMessage, HumanMessage

messages = [
    SystemMessage(content="You are a helpful Python tutor. Keep answers concise."),
    HumanMessage(content="How do I reverse a list in Python?")
]

response = llm.invoke(messages)
print(response.content)
# Output: You can reverse a list using slicing: my_list[::-1]
# Or use the built-in: my_list.reverse() (modifies in place)
```

---

## 6. Hands-on: Building a QnA Chatbot (Step by Step)

### Prerequisites
```bash
pip install langchain langchain-openai python-dotenv
```

### Step 1 — Load API Keys
```python
import os
from dotenv import load_dotenv

load_dotenv()
# OPENAI_API_KEY is now available in the environment
```

### Step 2 — Initialize the Model
```python
from langchain_openai import ChatOpenAI

# model: which GPT version to use
# temperature: 0.0 = deterministic/focused, 1.0 = creative/random
llm = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0.7,
    max_tokens=512
)
```

### Step 3 — Simple Single Question
```python
user_query = "What is the difference between Machine Learning and Generative AI?"

response = llm.invoke(user_query)

# response is an AIMessage object
print(type(response))         # <class 'langchain_core.messages.ai.AIMessage'>
print(response.content)       # The actual text answer
print(response.usage_metadata) # Token counts (input, output, total)
```

### Step 4 — Multi-turn Conversation with System Message
```python
from langchain_core.messages import SystemMessage, HumanMessage, AIMessage

# Build a conversation history
conversation = [
    SystemMessage(content="You are a knowledgeable AI teacher. Explain concepts clearly with analogies."),
    HumanMessage(content="What is an LLM?"),
]

# First turn
response1 = llm.invoke(conversation)
print("AI:", response1.content)

# Add the AI's response to the history and ask a follow-up
conversation.append(AIMessage(content=response1.content))
conversation.append(HumanMessage(content="Can you give me a real-world analogy?"))

# Second turn — the model remembers the previous context
response2 = llm.invoke(conversation)
print("AI:", response2.content)
```

### Step 5 — Switching to a Different Provider
Change just the import and initialization — everything else stays the same:

```python
# Switch from OpenAI to Groq (free and ultra-fast)
from langchain_groq import ChatGroq

llm = ChatGroq(model="llama3-8b-8192", temperature=0.7)

# Same code as before works without any changes!
response = llm.invoke("What is the difference between Machine Learning and Generative AI?")
print(response.content)
```

---

## 7. Understanding the AIMessage Response Object

When you call `.invoke()`, LangChain returns an **AIMessage** object — not just a plain string. It contains useful metadata:

```python
response = llm.invoke("Hello!")

print(response.content)          # "Hello! How can I help you today?"
print(response.id)               # Unique message ID
print(response.usage_metadata)   # {'input_tokens': 10, 'output_tokens': 9, 'total_tokens': 19}
print(response.response_metadata) # Model name, finish reason, etc.
```

To extract just the text, always use `response.content`.

---

## 8. Key Parameters Explained

### `temperature` — Controls Creativity
```python
# Low temperature: Consistent, factual answers (good for coding, Q&A)
llm_precise = ChatOpenAI(model="gpt-4o-mini", temperature=0.0)

# High temperature: Creative, varied answers (good for writing, brainstorming)
llm_creative = ChatOpenAI(model="gpt-4o-mini", temperature=1.0)

question = "Give me a name for an AI startup."

print(llm_precise.invoke(question).content)   # Will give similar answers each time
print(llm_creative.invoke(question).content)  # Will give different, creative answers each time
```

### `max_tokens` — Controls Response Length
```python
# Limit response to 50 tokens (roughly 35-40 words)
llm_short = ChatOpenAI(model="gpt-4o-mini", max_tokens=50)
response = llm_short.invoke("Explain quantum computing.")
print(response.content)  # Will be cut off at ~50 tokens
```

---

## 9. Summary of Learning
* **Portability**: Switch providers by changing one line, not rewriting entire logic.
* **Standardization**: Every response is an `AIMessage` with a `.content` attribute.
* **Message Roles**: Use `SystemMessage`, `HumanMessage`, and `AIMessage` to build structured conversations.
* **Parameters**: Control behavior with `temperature` (creativity) and `max_tokens` (length).
* **Foundation**: This setup is the base on which all future classes (Memory, RAG, Agents) are built.

---

## 10. Summary Checklist for Class 3
* [ ] Understand why LangChain solves the fragmentation problem.
* [ ] Know the four tools in the LangChain ecosystem.
* [ ] Know the 5 pillars of LangChain.
* [ ] Understand the three message roles: System, Human, AI.
* [ ] Build a single-turn Q&A call using `.invoke()`.
* [ ] Build a multi-turn conversation using a message list.
* [ ] Successfully switch the same code between two providers.

---

### 💡 Analogy: The Universal Remote Control

Imagine you have 5 TV brands in your house — Samsung, Sony, LG, Philips, Panasonic. Normally, each needs its own remote (its own SDK). **LangChain is the universal remote** — one set of buttons (`.invoke()`, `.content`) that controls all of them. You press the same "Volume Up" button regardless of which TV you are pointing at.
