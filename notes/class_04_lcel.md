# Class 4: Dynamic Prompts and LangChain Expression Language (LCEL)

In this session, we move beyond simple Q&A. We learn how to build **Professional AI Pipelines** by transitioning from static, hard-coded strings to **Dynamic Prompt Templates** and connecting components using **LCEL (LangChain Expression Language)** — the pipe operator that makes LangChain chains clean, readable, and powerful.

---

## 1. Static vs. Dynamic Prompts: The Engineering Gap

In Class 3, we sent a simple string directly to the LLM. That works for one-off questions, but real applications need to reuse the same structure with different data every time.

### The Problem with Static Prompts

```python
# ❌ Static — You have to manually rebuild this string every time
response = llm.invoke("Translate 'Hello' to Urdu")
response = llm.invoke("Translate 'Good morning' to French")
response = llm.invoke("Translate 'Thank you' to Spanish")
# This is unscalable — the structure is identical but the data changes!
```

### The Solution: Dynamic Prompt Templates

Instead of rebuilding the string, define the **structure once** using placeholders:

```python
# ✅ Dynamic — Define once, reuse infinitely with different values
template = "Translate '{text}' from {input_language} to {output_language}."

# Fill in the placeholders at runtime
filled = template.format(text="Hello", input_language="English", output_language="Urdu")
print(filled)
# Output: "Translate 'Hello' from English to Urdu."
```

LangChain's `ChatPromptTemplate` takes this concept further — it handles multiple message roles (System + Human) and fills all placeholders at once.

---

## 2. Deep Dive: ChatPromptTemplate

A **professional prompt** consists of distinct roles. LangChain's `ChatPromptTemplate` allows you to define both the system instruction and the user's input as separate, structured messages.

### Basic Structure:

```python
from langchain_core.prompts import ChatPromptTemplate

# Define the template with placeholders in curly braces {}
prompt_template = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant that translates {input_language} to {output_language}."),
    ("human", "{text}")
])

# Inspect what the template expects
print(prompt_template.input_variables)
# Output: ['input_language', 'output_language', 'text']
```

### Formatting the Prompt (for testing/debugging):

```python
# Fill in the placeholders and inspect the output
formatted = prompt_template.format_messages(
    input_language="English",
    output_language="French",
    text="I love programming with LangChain!"
)

for message in formatted:
    print(f"[{message.type}]: {message.content}")

# Output:
# [system]: You are a helpful assistant that translates English to French.
# [human]: I love programming with LangChain!
```

### Using `.invoke()` on the Template:

```python
# You can also call .invoke() with a dictionary
messages = prompt_template.invoke({
    "input_language": "English",
    "output_language": "Urdu",
    "text": "Good morning, how are you?"
})

print(messages)
```

---

## 3. LCEL: The Pipe Operator (`|`)

**LangChain Expression Language (LCEL)** is the core design pattern in modern LangChain. It uses Python's pipe operator (`|`) to connect components into a clean, linear pipeline.

### What is a "Runnable"?

Every LangChain component — Prompt, Model, Output Parser, or even a custom function — is a **Runnable**. A Runnable is anything that:
- Accepts input
- Processes it
- Returns output

Because they all follow the same interface, you can chain them with `|`.

### The Flow:

```
Input Dict → Prompt Template → LLM Model → Output Parser → Final Result
     |               |               |              |
  {"key": "val"}  AIMessage(...)  "Plain text"   Processed
```

### Building Your First Chain:

```python
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

load_dotenv()

# 1. Define each component separately
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant that translates {input_language} to {output_language}."),
    ("human", "{text}")
])

llm = ChatOpenAI(model="gpt-4o-mini", temperature=0.3)
parser = StrOutputParser()

# 2. Connect them with the pipe operator
translation_chain = prompt | llm | parser

# 3. Invoke the whole chain with one call
result = translation_chain.invoke({
    "input_language": "English",
    "output_language": "Urdu",
    "text": "The weather is beautiful today."
})

print(result)
# Output: "آج موسم بہت خوبصورت ہے۔"
```

---

## 4. Output Parsers — Extracting Clean Results

### A. StrOutputParser — The Most Common Parser

By default, the LLM returns an `AIMessage` object (with metadata, token counts, etc.). If your application only needs the plain text, use `StrOutputParser`:

```python
from langchain_core.output_parsers import StrOutputParser

# Without parser — you get the full AIMessage object
response = llm.invoke("What is 2 + 2?")
print(type(response))     # <class 'langchain_core.messages.ai.AIMessage'>
print(response.content)   # "4"

# With StrOutputParser in the chain — you get a plain string directly
chain = llm | StrOutputParser()
result = chain.invoke("What is 2 + 2?")
print(type(result))   # <class 'str'>
print(result)         # "4"
```

### B. Custom Functions as Pipeline Nodes

You can inject any Python function directly into the chain using the pipe operator:

```python
def to_uppercase(text: str) -> str:
    return text.upper()

def add_border(text: str) -> str:
    border = "=" * 40
    return f"\n{border}\n{text}\n{border}"

# Chain: prompt → llm → parser → custom function 1 → custom function 2
styled_chain = prompt | llm | StrOutputParser() | to_uppercase | add_border

result = styled_chain.invoke({
    "input_language": "English",
    "output_language": "French",
    "text": "Hello, world!"
})
print(result)
# Output:
# ========================================
# BONJOUR LE MONDE!
# ========================================
```

---

## 5. Full Professional Example: Recipe Generator

Let's build a complete, real-world chain that generates recipes and formats the output professionally:

```python
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

load_dotenv()

# --- Components ---
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0.8)
parser = StrOutputParser()

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are an expert chef from Pakistan. Give a 2-sentence recipe for {dish}. Use simple language."),
    ("human", "Make it spicy? {is_spicy}")
])

# --- Custom Post-Processing ---
def format_recipe(text: str) -> str:
    return f"🍽️  RECIPE RESULT\n{'─' * 40}\n{text}\n{'─' * 40}"

# --- Build the Chain ---
recipe_chain = prompt | llm | parser | format_recipe

# --- Run the Chain ---
result = recipe_chain.invoke({
    "dish": "Chicken Biryani",
    "is_spicy": "Yes, extra spicy!"
})

print(result)
```

**Expected Output:**
```
🍽️  RECIPE RESULT
────────────────────────────────────────
Marinate chicken in yogurt, red chili, and garam masala for 2 hours. Layer
with parboiled rice, fried onions, and saffron milk, then steam (dum) on low
heat for 30 minutes until fragrant and fully cooked.
────────────────────────────────────────
```

---

## 6. Batch Processing — Running Multiple Inputs at Once

One of LCEL's built-in superpowers is **batch processing**. Instead of running the chain one input at a time, you can process multiple inputs in parallel:

```python
# Single invoke — one input at a time
result = translation_chain.invoke({
    "input_language": "English",
    "output_language": "French",
    "text": "Hello"
})

# Batch invoke — multiple inputs processed in parallel
inputs = [
    {"input_language": "English", "output_language": "French", "text": "Hello"},
    {"input_language": "English", "output_language": "Urdu",   "text": "Good morning"},
    {"input_language": "English", "output_language": "Arabic", "text": "Thank you"},
]

results = translation_chain.batch(inputs)

for i, result in enumerate(results):
    print(f"Translation {i+1}: {result}")
```

---

## 7. Streaming — Real-time Token Output

For user-facing applications (like chatbots), streaming shows the response word-by-word as it is generated — instead of waiting for the full response:

```python
# Stream the output token by token
for chunk in translation_chain.stream({
    "input_language": "English",
    "output_language": "Urdu",
    "text": "Artificial Intelligence is changing the world."
}):
    print(chunk, end="", flush=True)  # Print each token as it arrives

print()  # New line when done
```

---

## 8. Why LCEL Architecture Matters

| Benefit | Without LCEL | With LCEL |
| :--- | :--- | :--- |
| **Readability** | `parser(llm(prompt(input)))` | `prompt \| llm \| parser` |
| **Modularity** | Tightly coupled functions | Swap any component independently |
| **Streaming** | Manual, complex to implement | Built-in via `.stream()` |
| **Batching** | Manual loops | Built-in via `.batch()` |
| **Debugging** | Print statements everywhere | LangSmith traces every step |

---

## 9. Summary Checklist for Class 4

* [ ] **Placeholders**: Use `{variable}` inside strings to define dynamic data points.
* [ ] **ChatPromptTemplate**: Use `from_messages()` to define System + Human roles separately.
* [ ] **Runnable**: Every LangChain component is a Runnable and can be chained.
* [ ] **The Pipe (`|`)**: Connect components left to right: `prompt | llm | parser`.
* [ ] **StrOutputParser**: Extract clean text from the AIMessage object.
* [ ] **Custom Functions**: Inject plain Python functions into the pipeline with `|`.
* [ ] **Batch**: Use `.batch([list of inputs])` to run multiple inputs in parallel.
* [ ] **Stream**: Use `.stream(input)` to display output in real-time.

---

### 💡 Analogy: The Car Factory Assembly Line

Think of building a car in a factory:

- **The Prompt Template** is the **chassis mold** — the same frame is used for every car.
- **The LLM** is the **engine installation station** — it powers the output.
- **The Output Parser** is the **quality check station** — it strips the raw output and delivers the finished product.
- **Custom Functions** are **specialized stations** (paint, windows, tires) that add extra processing.
- **LCEL (`|`)** is the **conveyor belt** — it moves the car automatically from one station to the next.

Without the conveyor belt, a worker would have to carry the car by hand to every station. With LCEL, everything flows automatically and in order.
