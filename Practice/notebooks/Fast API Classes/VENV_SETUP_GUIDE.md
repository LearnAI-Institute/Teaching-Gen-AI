# Virtual Environment Setup Guide

## ✅ Virtual Environment Created!

A Python virtual environment has been created for this FastAPI course.

### Location
```
Fast API Classes/venv/
```

### What's Installed
- ✅ **FastAPI** 0.135.3
- ✅ **Uvicorn** 0.44.0 (Server)
- ✅ **Pydantic** (Data validation)
- ✅ **Jupyter Notebook** (for interactive notebooks)

---

## How to Use the Virtual Environment

### On Windows (PowerShell or CMD)

#### Activate the virtual environment
```bash
# Using PowerShell
.\venv\Scripts\Activate.ps1

# Using CMD
venv\Scripts\activate
```

You should see `(venv)` at the start of your terminal line:
```
(venv) C:\Users\user\Documents\Teaching-Gen-AI\Practice\notebooks\Fast API Classes>
```

#### Run a FastAPI example
```bash
# Make sure you're in the FastAPI_Starter_Examples folder
cd FastAPI_Starter_Examples

# Run any example
uvicorn 01_hello:app --reload
uvicorn 02_hello_name:app --reload
uvicorn 05_calculator:app --reload
```

#### Run the Jupyter Notebook
```bash
# From the main Fast API Classes directory
jupyter notebook

# This opens a browser with the notebook interface
# Click on: Class_2_Week_2_FastAPI_Basics.ipynb
```

#### Deactivate the virtual environment
```bash
deactivate
```

---

### On Mac/Linux

#### Activate the virtual environment
```bash
source venv/bin/activate
```

#### Run a FastAPI example
```bash
cd FastAPI_Starter_Examples
uvicorn 01_hello:app --reload
```

#### Run the Jupyter Notebook
```bash
jupyter notebook
```

#### Deactivate the virtual environment
```bash
deactivate
```

---

## Quick Start Checklist

- [ ] Activate virtual environment: `.\venv\Scripts\activate` (Windows) or `source venv/bin/activate` (Mac/Linux)
- [ ] See `(venv)` in your terminal prompt
- [ ] Go to `FastAPI_Starter_Examples` folder
- [ ] Run: `uvicorn 01_hello:app --reload`
- [ ] Open browser: `http://localhost:8000/`
- [ ] See the response: `{"message":"Pakistan zinda bad!"}`
- [ ] Go to: `http://localhost:8000/docs` to see interactive docs
- [ ] Stop the server: `Ctrl+C`
- [ ] Deactivate environment: `deactivate`

---

## Using the Jupyter Notebook

### To Open the Notebook

```bash
# Activate venv first
.\venv\Scripts\activate

# Start Jupyter
jupyter notebook
```

This opens your browser to Jupyter. Then:
1. Click on `Class_2_Week_2_FastAPI_Basics.ipynb`
2. Read through the lessons
3. Run code cells with `Shift+Enter`
4. Complete the exercises

### In the Notebook

- **Markdown cells** = Explanations and concepts
- **Code cells** = Actual Python code you can run
- **Run a cell** = Click on it and press `Shift+Enter`
- **Create new cell** = Click `+` in toolbar

---

## Troubleshooting

### "The term 'venv' is not recognized"
**Windows Solution:**
```bash
# Make sure you're in the right directory
cd "C:\Users\user\Documents\Teaching-Gen-AI\Practice\notebooks\Fast API Classes"

# Then try again
.\venv\Scripts\activate
```

### "ModuleNotFoundError: No module named 'fastapi'"
**Solution:**
```bash
# Make sure venv is activated (should see "(venv)" in prompt)
.\venv\Scripts\activate

# Then reinstall
pip install fastapi uvicorn
```

### "Port 8000 already in use"
**Solution:**
```bash
# Use a different port
uvicorn 01_hello:app --port 8001 --reload
```

### "Jupyter command not found"
**Solution:**
```bash
# Make sure venv is activated
.\venv\Scripts\activate

# Install Jupyter
pip install jupyter notebook

# Then run
jupyter notebook
```

---

## Installing Additional Packages (If Needed)

If you need to install more packages in the future:

```bash
# Activate venv
.\venv\Scripts\activate

# Install package
pip install package_name

# For example
pip install requests
pip install pandas
pip install sqlalchemy
```

---

## File Structure

```
Fast API Classes/
├── venv/                          ← Virtual environment
│   ├── Scripts/
│   ├── Lib/
│   └── pyvenv.cfg
├── Class_2_Week_2_FastAPI_Basics.md      ← Markdown notes
├── Class_2_Week_2_FastAPI_Basics.ipynb   ← Jupyter Notebook (USE THIS!)
├── FASTAPI_QUICK_SETUP.md
├── VENV_SETUP_GUIDE.md            ← You are here
├── FastAPI_Starter_Examples/
│   ├── 01_hello.py
│   ├── 02_hello_name.py
│   ├── 03_search.py
│   ├── 04_create_user.py
│   ├── 05_calculator.py
│   ├── README.md
│   └── EXERCISE_SOLUTIONS.md
└── learn-agentic-ai-cloud/        ← Reference examples
```

---

## Important Notes

### About the Virtual Environment

1. **Why use it?** - Keeps packages organized per project
2. **Safe to delete** - You can delete `venv/` and recreate it anytime
3. **Must activate** - Always activate before using `pip` or `uvicorn`
4. **Size** - Takes up ~500MB (that's normal)

### About the Notebook

1. **Better for teaching** - Cells let you run code step by step
2. **Interactive** - You can modify and rerun code easily
3. **Documentation** - Can mix explanations and code
4. **Runs in browser** - No special installation needed (Jupyter is already here)

---

## Quick Command Reference

```bash
# Activate virtual environment (Windows)
.\venv\Scripts\activate

# Activate virtual environment (Mac/Linux)
source venv/bin/activate

# Deactivate
deactivate

# List installed packages
pip list

# Install a package
pip install package_name

# Run FastAPI example
cd FastAPI_Starter_Examples
uvicorn 01_hello:app --reload

# Open Jupyter Notebook
jupyter notebook

# Stop a running server
Ctrl+C
```

---

## Next Steps

1. **Read the Notebook**: Open `Class_2_Week_2_FastAPI_Basics.ipynb` in Jupyter
2. **Run the Examples**: Execute each code cell
3. **Try the Exercises**: Complete the practice problems in the notebook
4. **Test with Uvicorn**: Run the starter examples with uvicorn
5. **Complete Homework**: Build your own APIs for the exercises

---

## Getting Help

- **Setup Issues?** Check the FASTAPI_QUICK_SETUP.md file
- **Course Content?** Read Class_2_Week_2_FastAPI_Basics.md
- **Want to See Solutions?** Check FastAPI_Starter_Examples/EXERCISE_SOLUTIONS.md
- **Need Working Examples?** Go to FastAPI_Starter_Examples/ folder

---

**You're all set! Happy learning! 🚀**

Start with the Jupyter notebook:
```bash
.\venv\Scripts\activate
jupyter notebook
# Click on: Class_2_Week_2_FastAPI_Basics.ipynb
```
