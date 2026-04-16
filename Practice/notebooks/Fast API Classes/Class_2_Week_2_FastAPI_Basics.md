# Class 2, Week 2: FastAPI Fundamentals (Starting from Basics)

## Overview
In this class, we'll learn **FastAPI** from the ground up. FastAPI is a modern Python web framework that makes it easy to build APIs (Application Programming Interfaces) quickly.

### What You'll Learn
- What is an API and why it matters
- What is FastAPI
- How to create your first API endpoint
- Different ways to pass data to APIs (URLs, query parameters, request bodies)
- How to test your API

---

## 1. What is an API?

### Simple Definition
An **API (Application Programming Interface)** is a way for programs to talk to each other over the internet using HTTP requests and responses.

### Analogy
Think of a restaurant:
- **You** = Client (someone making a request)
- **Waiter** = API (takes your request)
- **Kitchen** = Server (processes your request)
- **Food** = Response (what you get back)

### HTTP Methods (The Basic Ones)
- **GET** → Ask for information (like reading data)
- **POST** → Send information (like creating new data)
- **PUT** → Update existing information
- **DELETE** → Remove information

---

## 2. What is FastAPI?

### In Simple Terms
FastAPI is a Python library that lets you create web APIs by writing just a few lines of code.

### Why FastAPI?
1. **Fast** → It's actually fast 🚀
2. **Easy** → Simple syntax, less code than other frameworks
3. **Automatic** → It automatically validates your data and creates documentation
4. **Modern** → Built with modern Python features

### Comparison with Basic Python
```python
# Traditional Python (without FastAPI)
# You'd need to manually:
# - Handle HTTP requests
# - Parse data
# - Validate inputs
# - Handle errors
# - Write documentation

# With FastAPI:
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, World!"}

# Done! It automatically handles everything.
```

---

## 3. Installation and Setup

### Step 1: Install FastAPI
```bash
pip install fastapi
pip install uvicorn  # This is a server that runs your API
```

### Step 2: Create Your First API File
Create a file called `main.py`:

```python
from fastapi import FastAPI

# Create an API application
app = FastAPI()

# Create an endpoint (a URL path you can access)
@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

### Step 3: Run Your API
```bash
uvicorn main:app --reload
```

### Step 4: Access Your API
- Open your browser and go to: `http://localhost:8000/`
- You should see: `{"message":"Hello, World!"}`

### Bonus: Automatic Documentation
- Go to: `http://localhost:8000/docs` (Swagger UI - interactive)
- Go to: `http://localhost:8000/redoc` (ReDoc - pretty docs)

---

## 4. Core Concepts

### 4.1 Routes and Decorators

A **route** is a URL path. A **decorator** (`@app.get()`) connects that path to a Python function.

```python
from fastapi import FastAPI

app = FastAPI()

# This says: "When someone visits http://localhost:8000/hello, run this function"
@app.get("/hello")
def say_hello():
    return {"message": "Hi there!"}

# Another route
@app.get("/goodbye")
def say_goodbye():
    return {"message": "See you later!"}
```

### 4.2 Path Parameters (Variables in the URL)

You can include variables in your URL:

```python
from fastapi import FastAPI

app = FastAPI()

# {name} is a variable - it captures whatever the user puts there
@app.get("/greet/{name}")
def greet(name: str):
    return {"message": f"Hello, {name}!"}

# Examples:
# http://localhost:8000/greet/Ali → {"message": "Hello, Ali!"}
# http://localhost:8000/greet/Fatima → {"message": "Hello, Fatima!"}
```

**Note:** The `: str` means the variable should be text. FastAPI validates this automatically.

### 4.3 Query Parameters (Variables after the `?`)

```python
from fastapi import FastAPI

app = FastAPI()

# http://localhost:8000/search?item=python
# http://localhost:8000/search?item=python&category=programming
@app.get("/search")
def search(item: str, category: str = "general"):
    return {
        "searching for": item,
        "in category": category
    }
```

**Difference:**
- **Path parameter:** `/greet/Ali` → built into the URL path
- **Query parameter:** `/search?item=Ali` → comes after the `?`

### 4.4 Request Body (Sending Data to the Server)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Define what data looks like (a model)
class User(BaseModel):
    name: str
    age: int
    email: str

# POST means "send new data to create something"
@app.post("/users")
def create_user(user: User):
    return {
        "message": f"User {user.name} created!",
        "user": user
    }
```

**How to test this:**
- You can't use a regular browser (browsers only do GET)
- Use the API docs at `http://localhost:8000/docs` or a tool like `curl` or Postman

---

## 5. Hands-On Examples

### Example 1: Simple Hello World
**File:** `01_hello.py`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Pakistan zinda bad!"}

# Run: uvicorn 01_hello:app --reload
# Visit: http://localhost:8000/
```

**What happens:**
- When you visit `/`, it returns JSON with your message

---

### Example 2: Hello with a Name
**File:** `02_hello_name.py`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/hello/{name}")
def hello(name: str):
    return {"message": f"Hello, {name}!"}

# Run: uvicorn 02_hello_name:app --reload
# Visit: http://localhost:8000/hello/Ali
# Visit: http://localhost:8000/hello/Fatima
```

---

### Example 3: Search with Query Parameters
**File:** `03_search.py`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/search")
def search(q: str):
    return {
        "you searched for": q,
        "results": ["result 1", "result 2"]
    }

# Run: uvicorn 03_search:app --reload
# Visit: http://localhost:8000/search?q=python
# Visit: http://localhost:8000/search?q=fastapi
```

---

### Example 4: Create a User (POST Request)
**File:** `04_create_user.py`

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Define user structure
class User(BaseModel):
    name: str
    age: int
    email: str

# Store users in memory (for now)
users = []

@app.post("/users")
def create_user(user: User):
    users.append(user)
    return {
        "message": f"User {user.name} created successfully!",
        "user": user
    }

@app.get("/users")
def get_users():
    return {"users": users}

# Run: uvicorn 04_create_user:app --reload
# Use http://localhost:8000/docs to test POST
```

---

### Example 5: Calculator API
**File:** `05_calculator.py`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/add")
def add(a: int, b: int):
    return {"result": a + b}

@app.get("/subtract")
def subtract(a: int, b: int):
    return {"result": a - b}

@app.get("/multiply")
def multiply(a: int, b: int):
    return {"result": a * b}

@app.get("/divide")
def divide(a: int, b: int):
    if b == 0:
        return {"error": "Cannot divide by zero"}
    return {"result": a / b}

# Run: uvicorn 05_calculator:app --reload
# Visit: http://localhost:8000/add?a=5&b=3
# Visit: http://localhost:8000/multiply?a=4&b=7
```

---

## 6. Testing Your API

### Method 1: Browser (for GET requests)
Just type the URL in your browser's address bar.

### Method 2: FastAPI Docs (for all requests)
1. Run your API: `uvicorn main:app --reload`
2. Go to: `http://localhost:8000/docs`
3. Click on an endpoint
4. Click "Try it out"
5. Enter values and click "Execute"

### Method 3: curl (command line)
```bash
# GET request
curl http://localhost:8000/hello/Ali

# POST request with data
curl -X POST http://localhost:8000/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Ali","age":25,"email":"ali@example.com"}'
```

---

## 7. Understanding Return Values

### Everything Becomes JSON
FastAPI automatically converts Python data to JSON:

```python
@app.get("/")
def example():
    # Python dict
    return {"name": "Ali", "age": 25}
    
# Becomes JSON:
# {"name": "Ali", "age": 25}
```

### Valid Return Types
- Dictionary: `{"key": "value"}`
- List: `[1, 2, 3]`
- String: `"hello"`
- Number: `42`
- Boolean: `True` or `False`
- Pydantic Model: `User(name="Ali", age=25)`

---

## 8. Common Errors & Solutions

### Error: "Module not found: fastapi"
```bash
pip install fastapi uvicorn
```

### Error: "Port 8000 already in use"
```bash
# Use a different port
uvicorn main:app --port 8001 --reload
```

### Error: "Cannot connect to http://localhost:8000"
- Make sure your API is still running (check your terminal)
- Make sure you used `--reload` flag

### Validation Error
```python
# Wrong - FastAPI will reject this if you require a string
@app.get("/user/{id}")
def get_user(id: str):
    return {"id": id}

# If you visit: /user/hello123 ✓ works
# If you visit: /user/123 ✓ also works (123 becomes "123")
```

---

## 9. Practice Exercises

### Exercise 1: Hello Name API
Create an API that:
- Has an endpoint `/greet/{name}`
- Returns a personalized greeting
- Example: `/greet/Ali` → `{"greeting": "Hello, Ali!"}`

### Exercise 2: Age Calculator
Create an API that:
- Has an endpoint `/age-in-years`
- Takes query parameter `birth_year`
- Returns how old someone is (approximate)
- Example: `/age-in-years?birth_year=2000` → `{"age": 24}`

### Exercise 3: Simple ToDo App
Create an API that:
- Has `/todos` GET endpoint that returns all todos
- Has `/todos` POST endpoint to add a new todo
- Returns todo data as JSON

### Exercise 4: Student Info
Create an API that:
- Takes a student name and subject via query parameters
- Returns a message like "Ali is studying Python"
- Example: `/student?name=Ali&subject=Python`

### Exercise 5: Math Quiz
Create an API that:
- Has endpoints for `/add`, `/subtract`, `/multiply`
- Each takes two numbers as query parameters
- Returns the result and shows the calculation
- Example: `/multiply?a=5&b=6` → `{"calculation": "5 * 6 = 30", "result": 30}`

---

## 10. Key Takeaways

1. **FastAPI creates web APIs with minimal code**
2. **@app.get() and @app.post() connect URLs to functions**
3. **Path parameters go in the URL: `/user/{id}`**
4. **Query parameters go after ?: `/search?q=python`**
5. **Request bodies send structured data via POST**
6. **FastAPI automatically validates and documents everything**
7. **Test with the docs at /docs or your browser**

---

## 11. Next Steps (What's Coming in Future Classes)

- Databases: Store data permanently (not just in memory)
- Authentication: Secure your APIs with login/passwords
- Error Handling: Better error messages
- Advanced Routing: Organizing large APIs
- Deployment: Put your API on the internet

---

## 12. Reference Examples (From the Repo)

The repository `learn-agentic-ai-cloud` has real working examples in:
- `07_fastapi/02_hello/` → Basic Hello World
- `07_fastapi/03_hello_who/` → URL paths, query parameters, request bodies
- `07_fastapi/04_pydantic/` → Data validation with Pydantic models

You can run these examples directly!

---

## Important Links

- **Official FastAPI Docs:** https://fastapi.tiangolo.com/learn/
- **Python Pydantic (for data validation):** https://docs.pydantic.dev/
- **HTTP Methods Reference:** https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods

---

**End of Class 2, Week 2**

**Homework:** Complete at least 3 of the practice exercises above. Be ready to show your working API in the next class!
