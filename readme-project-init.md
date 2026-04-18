# Project Initialization (Python Environment)

This project uses a **local Python virtual environment (`venv`)** to keep dependencies isolated and reproducible.

You only need to do this **once per machine**.

***

## 1. Create the Virtual Environment

From the project root:

```bash
python -m venv .venv
```

This creates a `.venv/` directory containing an isolated Python environment.

***

## 2. Activate the Virtual Environment

You must activate the venv **before installing dependencies or running the project**.

**Start environment:**
```
.\venv\Scripts\activate
```

**Stop environment:**
```
deactivate
```

## 3. Install Dependencies

With the venv activated:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

All required Python packages will now be installed **inside the virtual environment only**.

***

## 4. Verifying the Environment (Optional)

```bash
python --version
pip list
```

This should reflect the venv’s Python and installed packages.


# Key Notes

*   ✅ `requirements.txt` is the **single source of truth**
*   ✅ `.venv/` should **not** be committed to Git
*   ✅ Always activate the venv before running notebooks/scripts
*   ✅ Docker services (DB, Ollama) run independently of the venv
