### Week 2 Part 1: Create Your First FastAPI App

This tutorial shows you how to create your first FastAPI project in VS Code.

There are no videos for this week. Follow each step carefully and test your work in the browser.

#### What You Will Learn

- How to create a new FastAPI project.
- How to create and activate a virtual environment.
- How to use `requirements.txt`.
- How to run a FastAPI app with Uvicorn.
- How to test API endpoints in the browser.
- What a port is.

#### Part A: Create a New Project in VS Code

1. Open **Visual Studio Code**.

2. Create a new folder somewhere easy to find, for example in `Documents`.

   Name the folder:

```text
week2-hello-api
```

3. In VS Code, open the folder:

   **File > Open Folder**

4. Open a terminal inside VS Code:

   **Terminal > New Terminal**

5. Check that you are inside the project folder:

```bash
pwd
```

On Windows PowerShell, you can also use:

```powershell
Get-Location
```

#### Part B: Create a Virtual Environment

6. Create a virtual environment:

```bash
python3 -m venv .venv
```

On Windows, use one of these if `python3` does not work:

```powershell
python -m venv .venv
py -m venv .venv
```

7. Activate the virtual environment.

On macOS/Linux:

```bash
source .venv/bin/activate
```

On Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, run:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.venv\Scripts\Activate.ps1
```

When the environment is active, you should see `(.venv)` at the start of the terminal line.

> [!TIP]
>
> Why are we using a virtual environment again?
>
> <details>
> <summary>Show answer</summary>
>
> It keeps this project's Python packages separate from other projects. This helps avoid package version conflicts.
>
> </details>

#### Part C: Create `requirements.txt`

8. Create a new file in VS Code called:

```text
requirements.txt
```

9. Add these lines:

```text
fastapi
uvicorn
```

10. Save the file.

`requirements.txt` is a list of packages needed by the project. It helps everyone install the same dependencies.

11. Install the packages:

```bash
python -m pip install -r requirements.txt
```

12. Check the installed packages:

```bash
python -m pip list
```

You should see `fastapi` and `uvicorn`.

#### Part D: Create the FastAPI App

13. Create a new file called:

```text
main.py
```

14. Add this code:

```python
# Import FastAPI, the framework we use to build the API.
from fastapi import FastAPI

# Create the FastAPI application object.
# Uvicorn will run this object.
app = FastAPI()


# This endpoint runs when the browser opens http://127.0.0.1:8000/
@app.get("/")
def home():
    # Returning a dictionary sends JSON back to the browser.
    return {"message": "Hello world!"}


# This endpoint runs when the browser opens http://127.0.0.1:8000/hello
@app.get("/hello")
def hello():
    return {"message": "world!"}
```

15. Save the file.

#### What Is Happening in This Code?

- `FastAPI` creates the API application.
- `app = FastAPI()` creates one app object.
- `@app.get("/")` creates a GET endpoint for `/`.
- `@app.get("/hello")` creates a GET endpoint for `/hello`.
- `return {"message": "Hello world!"}` sends JSON back to the browser.

#### Part E: Run the API

16. Run the app from the VS Code terminal:

```bash
uvicorn main:app --reload
```

This means:

- `uvicorn` runs the server.
- `main` is the Python file `main.py`.
- `app` is the FastAPI object inside `main.py`.
- `--reload` restarts the server automatically when you save code changes.

17. Open your browser and visit:

```text
http://127.0.0.1:8000/
```

You should see:

```json
{"message":"Hello world!"}
```

18. Visit the second endpoint:

```text
http://127.0.0.1:8000/hello
```

You should see:

```json
{"message":"world!"}
```

#### What Is a Port?

A port is like a numbered door on your computer.

FastAPI is running on port `8000`, so the browser uses:

```text
http://127.0.0.1:8000
```

`127.0.0.1` means your own computer. It is also called `localhost`.

> [!TIP]
>
> Small check: if your FastAPI app is running on port `8000`, what browser URL opens the home endpoint?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/
> ```
>
> </details>

#### Part F: Stop the Server

19. Go back to the VS Code terminal.

20. Press:

```text
Ctrl + C
```

This stops the server.

Part 1 is complete. Continue to [Part 2](c2-02-part-2.md).
