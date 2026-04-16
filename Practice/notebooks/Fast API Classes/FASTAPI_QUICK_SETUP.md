# FastAPI Quick Setup Guide (For Class 2, Week 2)

## Prerequisites
- Python 3.7+ installed
- Basic knowledge of Python (variables, functions, dictionaries)
- A code editor (VS Code, PyCharm, etc.)
- Terminal/Command Prompt access

---

## Step-by-Step Setup (5 minutes)

### Step 1: Create a Project Folder
```bash
mkdir fastapi_learning
cd fastapi_learning
```

### Step 2: Create a Virtual Environment (Recommended)
```bash
# On Windows
python -m venv venv
venv\Scripts\activate

# On Mac/Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install FastAPI and Uvicorn
```bash
pip install fastapi uvicorn
```

**Verify installation:**
```bash
pip list | grep fastapi
pip list | grep uvicorn
```

### Step 4: Create Your First API File
Create a file named `main.py`:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

### Step 5: Run Your API
```bash
uvicorn main:app --reload
```

You should see:
```
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     Started server process
```

### Step 6: Test Your API
Open your browser and go to: **http://localhost:8000/**

You should see:
```json
{"message":"Hello, World!"}
```

**Congratulations! 🎉 Your first API is running!**

---

## Accessing the Interactive Documentation

Your API automatically has interactive documentation!

### Swagger UI (Interactive)
Go to: **http://localhost:8000/docs**
- Click on endpoints to see details
- Click "Try it out" to test them
- See real-time responses

### ReDoc (Beautiful docs)
Go to: **http://localhost:8000/redoc**
- Very clean documentation
- Good for sharing with others

---

## File Structure for This Course

```
fastapi_learning/
├── venv/                          # Virtual environment
├── FastAPI_Starter_Examples/      # Example files (from course)
│   ├── 01_hello.py
│   ├── 02_hello_name.py
│   ├── 03_search.py
│   ├── 04_create_user.py
│   ├── 05_calculator.py
│   └── README.md
├── my_exercises/                  # Your practice exercises
│   ├── exercise1_hello_api.py
│   ├── exercise2_age_calc.py
│   ├── exercise3_todo_app.py
│   └── ...
├── notes.md                       # Your notes
└── main.py                        # Your first API
```

---

## Running the Course Examples

All starter examples are in `FastAPI_Starter_Examples/` folder.

### Run Example 1 (Hello World)
```bash
cd FastAPI_Starter_Examples
uvicorn 01_hello:app --reload
# Visit: http://localhost:8000/
```

### Run Example 2 (Hello with Name)
```bash
uvicorn 02_hello_name:app --reload
# Visit: http://localhost:8000/hello/Ali
```

### Run Example 3 (Search)
```bash
uvicorn 03_search:app --reload
# Visit: http://localhost:8000/search?q=python
```

### Run Example 4 (Create User)
```bash
uvicorn 04_create_user:app --reload
# Visit: http://localhost:8000/docs and use the interface
```

### Run Example 5 (Calculator)
```bash
uvicorn 05_calculator:app --reload
# Visit: http://localhost:8000/add?a=5&b=3
```

---

## Common Terminal Commands

### Stop the API
```
Press Ctrl+C
```

### Run on Different Port
```bash
uvicorn main:app --port 8001 --reload
```

### Run Without Auto-Reload (Faster)
```bash
uvicorn main:app
```

### Make API Accessible from Other Machines
```bash
uvicorn main:app --host 0.0.0.0 --reload
```

### Check Python Version
```bash
python --version
# or
python3 --version
```

### List Installed Packages
```bash
pip list
```

---

## Troubleshooting

### Problem: "Command 'uvicorn' not found"
**Solution:**
```bash
pip install uvicorn
# OR use python module
python -m uvicorn main:app --reload
```

### Problem: "ModuleNotFoundError: No module named 'fastapi'"
**Solution:**
```bash
pip install fastapi
pip install fastapi uvicorn
```

### Problem: "Port 8000 already in use"
**Solution:** Use a different port
```bash
uvicorn main:app --port 8001 --reload
```

### Problem: "Virtual environment not activated"
**Solution:** Activate it
```bash
# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
```

You'll see `(venv)` at the start of your terminal line when it's active.

### Problem: "Permission denied" on Mac/Linux
**Solution:**
```bash
chmod +x venv/bin/activate
source venv/bin/activate
```

---

## Understanding the `--reload` Flag

When you use `uvicorn main:app --reload`:
- ✅ Server automatically restarts when you save changes
- ✅ Great for development
- ❌ Slower startup
- ❌ Don't use in production

---

## What's Happening When You Run `uvicorn main:app --reload`

```
uvicorn          # The server program
main             # Your Python file (main.py)
app              # The FastAPI object in your file
--reload         # Auto-restart on save
```

So it says: "Run the 'app' object from 'main.py' using Uvicorn, and reload on save"

---

## Quick Test Checklist

After installation, verify everything works:

- [ ] Python installed: `python --version`
- [ ] Virtual environment created and activated
- [ ] FastAPI installed: `pip show fastapi`
- [ ] Uvicorn installed: `pip show uvicorn`
- [ ] Created main.py with basic endpoint
- [ ] Started server: `uvicorn main:app --reload`
- [ ] Accessed http://localhost:8000/ in browser
- [ ] See JSON response in browser
- [ ] Accessed http://localhost:8000/docs in browser
- [ ] See interactive documentation

If all checks pass, you're ready to start learning FastAPI! ✅

---

## Important Points to Remember

1. **Always use a virtual environment** - keeps your packages organized
2. **Use `--reload` during development** - saves time fixing code
3. **Check the docs at /docs** - it's your best friend for testing
4. **FastAPI validates automatically** - don't worry about type checking
5. **Save your file, server restarts** - with --reload flag
6. **Keep terminal open** - shows errors and logs

---

## Next: Start with Course Materials

1. Read: **Class_2_Week_2_FastAPI_Basics.md**
2. Run: Examples from **FastAPI_Starter_Examples/**
3. Try: Practice exercises from the course notes
4. Build: Your own API from scratch

---

## Support Resources

- **Official FastAPI Docs:** https://fastapi.tiangolo.com/learn/
- **GitHub FastAPI:** https://github.com/tiangolo/fastapi
- **Course Repository:** learn-agentic-ai-cloud/ (in your directory)

---

**You're all set! Happy Learning! 🚀**

**Questions? Ask in class or review the course materials!**
