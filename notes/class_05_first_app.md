# Class 5: Building Your First AI Web App (Streamlit + LangChain)

In this session, we transition from Jupyter Notebooks to a **real, deployable web application**. We will build **AskBuddy** — a conversational AI chatbot with a professional chat interface, persistent message history, and Google Gemini as the backend — using **Streamlit** for the frontend and **LangChain** for the AI logic.

---

## 1. Notebooks vs. Apps: The Shift

So far, all our work has been inside `.ipynb` Jupyter notebooks — great for learning, but not usable by end-users. Real applications need clean, interactive web interfaces.

| Feature | Jupyter Notebooks (`.ipynb`) | Streamlit Apps (`.py`) |
| :--- | :--- | :--- |
| **Purpose** | Experimentation & Learning | Production & User Interaction |
| **Execution** | Cell by cell, manually | Entire script runs top-to-bottom automatically |
| **Interface** | Code-heavy, developer only | Clean web UI (buttons, chat, inputs) |
| **Shareable?** | Only with developers | Yes — with anyone via a browser |
| **Deployment** | Cannot deploy | Can deploy to Streamlit Cloud, AWS, etc. |

---

## 2. What is Streamlit?

**Streamlit** is an open-source Python library that lets you build interactive web applications **entirely in Python** — no HTML, CSS, or JavaScript required. It is the fastest way to turn a Python script into a shareable web app.

### How Streamlit Works (The Re-run Model)

This is the most important concept to understand about Streamlit:

> **Every time a user interacts with the app (clicks a button, types something), Streamlit re-runs the ENTIRE Python script from top to bottom.**

```
User types a message
        │
        ▼
Streamlit re-runs the whole script
        │
        ▼
All variables are reset to default
        │
        ▼
⚠️  Previous messages are LOST unless saved in st.session_state
```

This is why **`st.session_state`** is critical — it is the only way to preserve data across re-runs.

### Core Streamlit Commands Used in This App:

| Command | What it Does |
| :--- | :--- |
| `st.title("text")` | Displays a large heading |
| `st.markdown("text")` | Displays styled markdown text |
| `st.chat_input("placeholder")` | Renders the modern chat input box at the bottom |
| `st.chat_message("role")` | Creates a chat bubble for "user" or "ai" |
| `st.session_state` | A dictionary that persists data between re-runs |

---

## 3. Application Architecture

Our app follows a **4-step pipeline** on every message:

```
┌─────────────────────────────────────────────────────────────┐
│                    AskBuddy — App Flow                      │
│                                                             │
│  1. DISPLAY          2. USER INPUT       3. AI LOGIC        │
│  ─────────────       ────────────────    ──────────────     │
│  Re-render all  →    st.chat_input()  →  llm.invoke()       │
│  past messages       captures query      sends to Gemini    │
│  from session                                               │
│  state                                    4. SAVE & SHOW    │
│                                          ──────────────     │
│                                          Save both messages │
│                                          to session_state   │
│                                          Show response      │
└─────────────────────────────────────────────────────────────┘
```

---

## 4. Full Code Walkthrough — Line by Line

Here is the complete app code (`Practice/apps/1_qna_bot.py`), explained in detail:

```python
from dotenv import load_dotenv
load_dotenv()
```
> Loads the `.env` file so the `GOOGLE_API_KEY` is available as an environment variable. This must be called **before** any LLM is initialized.

---

```python
from langchain_google_genai import ChatGoogleGenerativeAI
import streamlit as st
```
> Imports the Google Gemini integration from LangChain and the Streamlit library.

---

```python
llm = ChatGoogleGenerativeAI(model="gemini-2.5-flash")
```
> Initializes the **Gemini 2.5 Flash** model. LangChain automatically picks up the `GOOGLE_API_KEY` from the environment. No temperature is set here, so it uses the model's default.

> **Why Gemini instead of OpenAI?** Gemini offers a generous free tier — you do not need to add billing credits to use it for learning.

---

```python
st.title("🤖 AskBuddy – AI QnA Bot")
st.markdown("My QnA Bot with LangChain and Google Gemini !")
```
> Renders the app title and subtitle at the top of the page. These run on every re-run (which is fine — they are static).

---

### The Most Important Block: `st.session_state`

```python
if "messages" not in st.session_state:
    st.session_state.messages = []
```

This is the **memory system** of the app. Here is what happens without it vs. with it:

**Without session_state:**
```
User sends "What is AI?"   → Gemini responds → messages = [...]
User sends "Tell me more." → Script re-runs  → messages = []  ← RESET! History lost!
```

**With session_state:**
```
User sends "What is AI?"   → messages saved to session_state
User sends "Tell me more." → Script re-runs → session_state.messages still has history ✅
```

The `if "messages" not in st.session_state` check ensures:
- On **first load**: the list is created empty.
- On **every re-run after that**: the existing list is preserved (not overwritten).

`st.session_state.messages` stores messages in this format:
```python
[
    {"role": "user", "content": "What is AI?"},
    {"role": "ai",   "content": "AI stands for Artificial Intelligence..."},
    {"role": "user", "content": "Give me an example."},
    {"role": "ai",   "content": "A chatbot like me is an example of AI!"},
]
```

---

### Rendering Chat History

```python
for message in st.session_state.messages:
    role = message["role"]
    content = message["content"]
    st.chat_message(role).markdown(content)
```

Every time the script re-runs, this loop **re-draws all previous messages** from `session_state` so the user always sees the full conversation history.

`st.chat_message("user")` renders a human bubble, and `st.chat_message("ai")` renders an AI bubble — Streamlit handles the styling automatically.

---

### Capturing User Input

```python
query = st.chat_input("Ask anything ?")
```

`st.chat_input()` renders the modern, sticky input bar at the **bottom** of the screen (like WhatsApp or ChatGPT). It returns the user's text when they press Enter, or `None` if they haven't typed anything yet.

---

### Processing and Responding

```python
if query:
    st.session_state.messages.append({"role": "user", "content": query})
    st.chat_message("user").markdown(query)

    res = llm.invoke(query)

    st.chat_message("ai").markdown(res.content)
    st.session_state.messages.append({"role": "ai", "content": res.content})
```

This block runs **only when the user has submitted a question**. Here is the step-by-step logic:

| Step | Code | What Happens |
| :--- | :--- | :--- |
| 1 | `messages.append({"role": "user", ...})` | Save user message to history |
| 2 | `st.chat_message("user").markdown(query)` | Show user bubble immediately |
| 3 | `llm.invoke(query)` | Send question to Gemini and wait for response |
| 4 | `st.chat_message("ai").markdown(res.content)` | Show AI response bubble |
| 5 | `messages.append({"role": "ai", ...})` | Save AI response to history |

> **Note:** `res` is an `AIMessage` object (as we learned in Class 3). We use `.content` to extract just the text.

---

## 5. Complete Annotated Code

```python
# ── Step 1: Load API Keys ────────────────────────────────────────────────────
from dotenv import load_dotenv
load_dotenv()  # Reads .env and loads GOOGLE_API_KEY into environment

# ── Step 2: Imports ──────────────────────────────────────────────────────────
from langchain_google_genai import ChatGoogleGenerativeAI
import streamlit as st

# ── Step 3: Initialize the AI Model ─────────────────────────────────────────
llm = ChatGoogleGenerativeAI(model="gemini-2.5-flash")

# ── Step 4: Page Header ──────────────────────────────────────────────────────
st.title("🤖 AskBuddy – AI QnA Bot")
st.markdown("My QnA Bot with LangChain and Google Gemini !")

# ── Step 5: Initialize Chat History (runs only on first load) ────────────────
if "messages" not in st.session_state:
    st.session_state.messages = []  # Empty list to hold all messages

# ── Step 6: Re-render All Previous Messages ──────────────────────────────────
for message in st.session_state.messages:
    role = message["role"]       # "user" or "ai"
    content = message["content"] # The actual text
    st.chat_message(role).markdown(content)  # Render the chat bubble

# ── Step 7: Get New User Input ───────────────────────────────────────────────
query = st.chat_input("Ask anything ?")  # Returns text when submitted, else None

# ── Step 8: Process Input and Generate Response ──────────────────────────────
if query:  # Only runs if user submitted something
    # Save + show user message
    st.session_state.messages.append({"role": "user", "content": query})
    st.chat_message("user").markdown(query)

    # Call Gemini and get response
    res = llm.invoke(query)  # res is an AIMessage object

    # Show + save AI response
    st.chat_message("ai").markdown(res.content)  # .content extracts the text
    st.session_state.messages.append({"role": "ai", "content": res.content})
```

---

## 6. How to Run the App

Unlike standard Python scripts, Streamlit apps use their own CLI command:

```bash
# Make sure your venv is active first
streamlit run Practice/apps/1_qna_bot.py
```

This will:
1. Start a local web server
2. Automatically open your browser at `http://localhost:8501`
3. Display your app

To stop the app, press `Ctrl + C` in the terminal.

---

## 7. What This App Does vs. What It Does NOT Do (Yet)

| Feature | This App | Future Improvement |
| :--- | :--- | :--- |
| Remembers messages **visually** | ✅ Yes — via `session_state` | — |
| Remembers conversation **context for Gemini** | ❌ No — sends only the latest query | Class 6: Add Memory |
| Uses a Prompt Template | ❌ No — sends raw text | Can add `ChatPromptTemplate` |
| Streams response word-by-word | ❌ No — waits for full response | Can add `.stream()` |
| Multiple model choices | ❌ No — only Gemini | Can add a sidebar selector |

> **Important distinction:** `st.session_state` stores the **visual chat history** (what the user sees on screen). But currently, each `llm.invoke(query)` call only sends the **latest question** — Gemini does not "remember" the previous messages. This will be solved in Class 6 with LangChain Memory.

---

## 8. Summary Checklist for Class 5

* [ ] Understand why Streamlit re-runs the script on every interaction.
* [ ] Understand what `st.session_state` is and why it is necessary.
* [ ] Know the difference between `st.chat_input()` and `st.text_input()`.
* [ ] Know what `st.chat_message("role")` does.
* [ ] Read and understand every line of `1_qna_bot.py`.
* [ ] Successfully run the app with `streamlit run`.
* [ ] Understand what `.content` extracts from the `AIMessage` response.
* [ ] Know the current limitation: Gemini does not yet see conversation history.

---

### 💡 Analogy: The Restaurant

- **Streamlit** is the **Waiter** — takes the customer's order (user input), brings it to the kitchen, and delivers the food back (AI response) to the table.
- **`st.session_state`** is the **Waiter's notepad** — without it, the waiter would forget the entire previous order every time they walked to the kitchen and back.
- **LangChain + Gemini** is the **Kitchen and Chef** — receives the order, prepares the response, and sends it back out.
- **`st.chat_message()`** is the **plate presentation** — displays the food (text) in the right format depending on who ordered it (user bubble vs. AI bubble).
