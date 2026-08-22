### Week 1 Part 2: Hello FastAPI Exercise

This is a short exercise based on [Part 1](part-1.md).

Start from zero. Close your previous VSCode project first, then create a new project.

Solutions are in the [solutions](../solutions/) folder.

#### Goal

Create a new FastAPI app with two simple endpoints:

- `GET /` returns a welcome message
- `GET /about` returns information about the app

#### Part A: Close the Previous Project

1. In VS Code, close the folder from Part 1.

   Use:

```text
File > Close Folder
```

2. Create a new folder called:

```text
week1-exercise-api
```

3. Open the new folder in VS Code.

4. Open a new VS Code terminal.

#### Part B: Create the Python Environment

5. Create a virtual environment:

> [!TIP]
>
> Do you remember the command that creates a virtual environment called `.venv`?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> python3 -m venv .venv
> ```
>
> On Windows, use:
>
> ```powershell
> python -m venv .venv
> ```
>
> </details>

6. Activate the virtual environment.

> [!TIP]
>
> Do you remember how to activate `.venv`?
>
> <details>
> <summary>Show answer</summary>
>
> On macOS/Linux:
>
> ```bash
> source .venv/bin/activate
> ```
>
> On Windows PowerShell:
>
> ```powershell
> .venv\Scripts\Activate.ps1
> ```
>
> </details>

7. Create a file called:

```text
requirements.txt
```

8. Add:

```text
fastapi
uvicorn
```

9. Install the requirements:

> [!TIP]
>
> Do you remember how to install everything listed in `requirements.txt`?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> python -m pip install -r requirements.txt
> ```
>
> </details>

#### Part C: Create the App

10. Create a file called:

```text
main.py
```

11. Write a FastAPI app that has these endpoints:

```text
GET /
GET /about
```

The home endpoint should return:

```json
{"message": "Hello from my exercise API"}
```

The about endpoint should return:

```json
{
  "name": "Week 1 Exercise API",
  "version": "1.0.0",
  "author": "your name"
}
```

Replace `your name` with your own name.

#### Part D: Run and Test

12. Run the app:

> [!TIP]
>
> Do you remember the Uvicorn command that runs `main.py` and the `app` object?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> uvicorn main:app --reload
> ```
>
> </details>

13. Open the home endpoint in your browser:

> [!TIP]
>
> Do you remember the local browser URL for the home endpoint?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/
> ```
>
> </details>

14. Open the about endpoint:

> [!TIP]
>
> If the home endpoint is `/`, what URL should you use for `/about`?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/about
> ```
>
> </details>

15. Stop the server with:

> [!TIP]
>
> Do you remember the keyboard shortcut that stops the running server?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> Ctrl + C
> ```
>
> </details>

#### Checklist

Before you finish, make sure:

- you created a new project folder
- your virtual environment works
- `requirements.txt` contains `fastapi` and `uvicorn`
- `GET /` works in the browser
- `GET /about` works in the browser

Part 2 is complete. Continue to [Part 3](part-3.md).
