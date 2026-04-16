# 🚀 Class 2, Week 2: FastAPI Basics - Complete Setup

## ✅ Everything Is Ready!

You now have a **complete, production-ready FastAPI course** for teaching students from basics.

---

## 📁 What You Have

### 1. **Interactive Jupyter Notebook** (Main Teaching Resource)
📄 [`Class_2_Week_2_FastAPI_Basics.ipynb`](Class_2_Week_2_FastAPI_Basics.ipynb) ⭐ **USE THIS IN CLASS**

- Complete course material in interactive format
- 14 sections covering all concepts
- Live code examples students can run
- Practice exercises with hints
- Runs in your browser with Jupyter

**To open it:**
```bash
# From the Fast API Classes directory
.\venv\Scripts\activate              # Activate virtual environment
jupyter notebook                     # Start Jupyter
# Click on: Class_2_Week_2_FastAPI_Basics.ipynb
```

---

### 2. **Markdown Course Notes** (For Reference)
📄 [`Class_2_Week_2_FastAPI_Basics.md`](Class_2_Week_2_FastAPI_Basics.md)

- Detailed explanations with examples
- All concepts broken down simply
- Can be printed or shared with students
- Great for reference material

---

### 3. **5 Ready-to-Run Examples**
📁 [`FastAPI_Starter_Examples/`](FastAPI_Starter_Examples/)

Complete working code students can copy and modify:

```
01_hello.py              ← Simple GET endpoint
02_hello_name.py         ← Path parameters
03_search.py             ← Query parameters
04_create_user.py        ← POST requests & data validation
05_calculator.py         ← Multiple endpoints
```

**Run any example:**
```bash
cd FastAPI_Starter_Examples
uvicorn 01_hello:app --reload
# Visit: http://localhost:8000/
```

---

### 4. **Exercise Solutions & Grading**
📄 [`FastAPI_Starter_Examples/EXERCISE_SOLUTIONS.md`](FastAPI_Starter_Examples/EXERCISE_SOLUTIONS.md)

- Complete solutions to 5 exercises
- 3 bonus challenges
- Grading rubric (100 points)
- Common student mistakes & fixes

---

### 5. **Setup & Reference Guides**
📄 `FASTAPI_QUICK_SETUP.md` - Installation instructions  
📄 `VENV_SETUP_GUIDE.md` - Virtual environment usage  
📄 [`FastAPI_Starter_Examples/README.md`](FastAPI_Starter_Examples/README.md) - How to run examples

---

### 6. **Virtual Environment (Ready to Use)**
📁 `venv/` folder

Already installed:
- ✅ **FastAPI** 0.135.3
- ✅ **Uvicorn** 0.44.0
- ✅ **Pydantic** 2.13.1 (Data validation)
- ✅ **Jupyter Notebook** 7.5.5
- ✅ **Python** 3.14.1

---

## 🎯 How to Teach This Class

### **Before Class (Preparation)**

1. ✅ Virtual environment is already set up
2. ✅ All packages installed
3. ✅ All materials ready

### **During Class (Teaching Flow)**

#### **Part 1: Concepts (20 minutes)**
1. Open the Jupyter notebook: `Class_2_Week_2_FastAPI_Basics.ipynb`
2. Work through Sections 1-4:
   - What is an API?
   - What is FastAPI?
   - Installation verification
   - Core concepts (routes, parameters, body)

#### **Part 2: Live Demo (20 minutes)**
1. Stay in the Jupyter notebook
2. Run the 5 examples (Sections 5)
3. Students can follow along in their own notebooks
4. Discuss what each example does

#### **Part 3: Explanation (15 minutes)**
1. Discuss testing methods (Section 6)
2. Explain JSON format (Section 7)
3. Review common errors (Section 8)

#### **Part 4: Exercises (25 minutes)**
1. Students work on exercises 1-3 (minimum)
2. Teacher circulates to help
3. Discuss solutions after

#### **Total Time: 80 minutes**

---

## 📚 Student Materials to Share

### **Essential Files:**
1. **Notebook**: `Class_2_Week_2_FastAPI_Basics.ipynb` ← Open this in Jupyter
2. **Markdown**: `Class_2_Week_2_FastAPI_Basics.md` ← Print or read
3. **Examples**: `FastAPI_Starter_Examples/` ← Copy and run

### **How Students Should Use It:**

```bash
# Step 1: Activate virtual environment
.\venv\Scripts\activate

# Step 2: Open Jupyter notebook (recommended for class)
jupyter notebook
# Then click: Class_2_Week_2_FastAPI_Basics.ipynb

# OR Step 2: Run starter examples
cd FastAPI_Starter_Examples
uvicorn 01_hello:app --reload
# Visit: http://localhost:8000/
```

---

## 🔍 Key Features

### ✅ **Jupyter Notebook Advantages**
- Students can run code **in the browser** (no extra setup)
- They can **modify and experiment** with code
- **Markdown + Code** mixed together for better learning
- Can **save progress** within the notebook
- Great for **interactive teaching**

### ✅ **Starter Examples Advantages**
- **Real files** they can run standalone
- Can use with **any editor**
- Great for **homework**
- Can test with **full Uvicorn server**
- Access **interactive docs** at `/docs`

### ✅ **Multiple Reference Materials**
- Jupyter notebook for interactive learning
- Markdown notes for detailed reference
- Working code examples to copy
- Solutions for checking answers
- Setup guides for troubleshooting

---

## 📊 Class Schedule

| Time | Activity | Resource |
|------|----------|----------|
| 0-20 min | Teach concepts | Notebook Sections 1-4 |
| 20-40 min | Live demo examples | Notebook Section 5 |
| 40-55 min | Explain testing & errors | Notebook Sections 6-8 |
| 55-80 min | Exercise time | Notebook Section 10 |

---

## 🎓 What Students Learn

By the end of this class, students will understand:

- ✅ What an API is and why they matter
- ✅ What FastAPI is and why it's great
- ✅ How to create basic endpoints
- ✅ Path parameters (`/user/{id}`)
- ✅ Query parameters (`/search?q=python`)
- ✅ POST requests with data validation
- ✅ JSON responses
- ✅ How to test APIs (browser, `/docs`, curl)
- ✅ How to handle errors

---

## 📝 Homework Assignment

**Students must complete at least 3 of 5 exercises:**

1. Hello Name API (`/greet/{name}`)
2. Age Calculator (`/age-in-years?birth_year=2000`)
3. Simple Todo App (`POST /todos`, `GET /todos`)
4. Student Info (`/student?name=X&subject=Y`)
5. Math Quiz (`/add`, `/subtract`, `/multiply`, `/divide`)

**Deliverables:**
- Working Python files (one per exercise)
- API runs without errors
- Returns correct JSON format
- Demonstrates understanding of concepts

---

## 🔧 Technical Details

### **Virtual Environment**
- **Location**: `Fast API Classes/venv/`
- **Python Version**: 3.14.1
- **Packages Installed**: FastAPI, Uvicorn, Pydantic, Jupyter
- **Size**: ~500MB (normal)

### **How to Activate**

**Windows:**
```bash
.\venv\Scripts\activate
```

**Mac/Linux:**
```bash
source venv/bin/activate
```

### **Verify Setup**
```bash
# Check if virtual environment is active (should see "(venv)" in prompt)
pip list | grep fastapi
# Should show: fastapi    0.135.3
```

---

## 🆘 Troubleshooting (For You or Students)

### **Problem: "Module not found: fastapi"**
```bash
# Activate venv and reinstall
.\venv\Scripts\activate
pip install fastapi uvicorn
```

### **Problem: "Port 8000 already in use"**
```bash
# Use different port
uvicorn 01_hello:app --port 8001 --reload
```

### **Problem: "Cannot open Jupyter notebook"**
```bash
# Install Jupyter in the venv
.\venv\Scripts\activate
pip install jupyter notebook
jupyter notebook
```

### **Problem: "Uvicorn command not found"**
```bash
# Make sure venv is activated (see "(venv)" in prompt)
# If not activated:
.\venv\Scripts\activate
# Then try again
uvicorn 01_hello:app --reload
```

---

## 📖 Reference Material

### **For You (Teacher)**
- `Class_2_Week_2_FastAPI_Basics.md` - Full detailed guide
- `FastAPI_Starter_Examples/EXERCISE_SOLUTIONS.md` - Answer key
- `VENV_SETUP_GUIDE.md` - Setup troubleshooting

### **For Students**
- `Class_2_Week_2_FastAPI_Basics.ipynb` - Main interactive course
- `Class_2_Week_2_FastAPI_Basics.md` - Detailed reference
- `FastAPI_Starter_Examples/` - Working code to study
- `FastAPI_QUICK_SETUP.md` - How to get started
- `FastAPI_Starter_Examples/README.md` - How to run examples

### **External Resources**
- **Official Docs**: https://fastapi.tiangolo.com/learn/
- **Pydantic**: https://docs.pydantic.dev/
- **HTTP Methods**: https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods

---

## ✨ Pro Tips for Teaching

### 1. **Use the Jupyter Notebook**
- Students can follow along on their own laptops
- They can run code cells in real-time
- Pause and let them catch up
- Great for explaining concepts

### 2. **Live Coding**
- Show Examples 1-3 running in the Jupyter notebook
- Have students modify the code while you watch
- Show `/docs` endpoint for testing

### 3. **Interactive Testing**
- Open `http://localhost:8000/docs` on your screen
- Students see real API responses
- They understand the API → Browser relationship

### 4. **Encourage Experimentation**
- Have them modify the code
- Change messages, add parameters
- See what breaks and why

### 5. **Show Errors**
- Don't hide from errors
- Show common mistakes
- Explain what the error message means
- Students learn debugging this way

### 6. **Take It Slow**
- Don't rush through concepts
- Let students ask questions
- Make sure they understand before moving on
- FastAPI basics is not a lot of material

---

## 🚀 Quick Start Commands

```bash
# Activate virtual environment
.\venv\Scripts\activate

# Option 1: Use Jupyter Notebook (Recommended for teaching)
jupyter notebook
# Click: Class_2_Week_2_FastAPI_Basics.ipynb

# Option 2: Run starter examples
cd FastAPI_Starter_Examples
uvicorn 01_hello:app --reload
# Visit: http://localhost:8000/

# Option 3: Test with docs
# After running any example, go to:
# http://localhost:8000/docs

# Deactivate when done
deactivate
```

---

## 📋 Checklist Before Class

- [ ] Virtual environment activated: `.\venv\Scripts\activate`
- [ ] Jupyter installed: Check in previous setup
- [ ] Jupyter starts: `jupyter notebook`
- [ ] Notebook opens: `Class_2_Week_2_FastAPI_Basics.ipynb`
- [ ] Can run code cells in notebook
- [ ] Can run starter examples: `uvicorn 01_hello:app --reload`
- [ ] `/docs` endpoint works: `http://localhost:8000/docs`
- [ ] All example files present in `FastAPI_Starter_Examples/`
- [ ] Reference materials visible in file explorer

---

## 📞 Need Help?

### **Setup Issues?**
→ Read `VENV_SETUP_GUIDE.md`

### **Jupyter Problems?**
→ Run: `pip install jupyter notebook`

### **FastAPI Questions?**
→ Check `Class_2_Week_2_FastAPI_Basics.md`

### **Want to See Solutions?**
→ Check `FastAPI_Starter_Examples/EXERCISE_SOLUTIONS.md`

### **Students Stuck?**
→ Suggest they:
1. Read the course notes
2. Copy a starter example
3. Modify it gradually
4. Test in `/docs` endpoint

---

## 🎯 Summary

You have **everything needed** to teach FastAPI to students with basic Python knowledge:

1. ✅ **Interactive Jupyter Notebook** - Main teaching material
2. ✅ **Complete Course Notes** - For reference
3. ✅ **5 Working Examples** - Students can run & modify
4. ✅ **Virtual Environment** - All packages pre-installed
5. ✅ **Exercise Solutions** - Answer key for grading
6. ✅ **Setup Guides** - Troubleshooting help
7. ✅ **Reference Materials** - Multiple formats

---

## 🏁 Start Teaching!

**Quick start:**

```bash
cd "Fast API Classes"
.\venv\Scripts\activate
jupyter notebook
# Click: Class_2_Week_2_FastAPI_Basics.ipynb
```

**That's it! You're ready to teach FastAPI! 🚀**

---

**Questions? Review the course materials or check the setup guides above.**

**Good luck teaching! 🎓**
