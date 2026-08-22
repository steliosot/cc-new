### Week 1 Homework 1: University API Exercise

Now build a small University API using FastAPI.

Create a new project in VS Code. Do not reuse the movie project from Lab Part 3.

Solutions are in the [solutions](../solutions/) folder.

#### Goal

Your API should support these endpoints:

- `GET /` returns a welcome page with API information.
- `GET /students` returns all students.
- `GET /students/{student_id}` returns one student by ID.
- If the student does not exist, return a `404` JSON error.

You should use:

- `main.py`
- a `routers` folder
- `routers/students.py`
- `APIRouter`
- `requirements.txt`
- `uvicorn main:app --reload`

#### Part A: Create the Project

1. Create a new folder called:

```text
week1-university-api
```

2. Open it in VS Code.

3. Open a VS Code terminal.

4. Create and activate a virtual environment.

> [!TIP]
>
> Do you remember how to create and activate `.venv`?
>
> <details>
> <summary>Show answer</summary>
>
> On macOS/Linux:
>
> ```bash
> python3 -m venv .venv
> source .venv/bin/activate
> ```
>
> On Windows PowerShell:
>
> ```powershell
> python -m venv .venv
> .venv\Scripts\Activate.ps1
> ```
>
> </details>

5. Create a `requirements.txt` file with:

```text
fastapi
uvicorn
```

6. Install the requirements:

> [!TIP]
>
> Do you remember how to install the packages listed in `requirements.txt`?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> python -m pip install -r requirements.txt
> ```
>
> </details>

#### Part B: Create the Files

Create this structure:

```text
week1-university-api/
  main.py
  requirements.txt
  routers/
    __init__.py
    students.py
```

> [!TIP]
>
> Why do we create `routers/students.py` instead of writing every endpoint in `main.py`?
>
> <details>
> <summary>Show answer</summary>
>
> It keeps the API organised. Student-related routes live in one file, and the main app only connects the router.
>
> </details>

#### Part C: Root Endpoint

Your root endpoint should return:

```json
{
  "name": "University API",
  "version": "1.0.0",
  "description": "A simple API to fetch university student data.",
  "endpoints": {
    "root": "GET /",
    "all_students": "GET /students",
    "student_by_id": "GET /students/s1"
  }
}
```

#### Part D: Student Data

Use this data in `routers/students.py`:

```python
students = {
    # The keys s1, s2, and s3 are the IDs used in the URL.
    "s1": {
        "name": "John Smith",
        "student_id": "S12345",
        "major": "Computer Science",
        "year": 2
    },
    "s2": {
        "name": "Maria Garcia",
        "student_id": "S54321",
        "major": "Mathematics",
        "year": 3
    },
    "s3": {
        "name": "Ali Khan",
        "student_id": "S77777",
        "major": "Data Science",
        "year": 1
    }
}
```

#### Part E: Required Tests

Run the server:

> [!TIP]
>
> Do you remember the command that runs the FastAPI app?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> uvicorn main:app --reload
> ```
>
> </details>

Open these URLs in your browser:

> [!TIP]
>
> Do you remember the local URL for the root endpoint?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/
> ```
>
> </details>

> [!TIP]
>
> What URL should show all students?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/students
> ```
>
> </details>

> [!TIP]
>
> What URL should show student `s1`?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/students/s1
> ```
>
> </details>

> [!TIP]
>
> What URL should test the `404` error for a missing student?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/students/s10
> ```
>
> </details>

The final URL should return a `404` error with:

```json
{"detail":"Student not found"}
```

> [!TIP]
>
> Your students router uses `@router.get("/{student_id}")`, and `main.py` should use `prefix="/students"`. What URL returns student `s2`?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> http://127.0.0.1:8000/students/s2
> ```
>
> </details>

#### Hints

In `main.py`, you will need:

```python
from fastapi import FastAPI

# Import the students router from routers/students.py.
from routers import students

app = FastAPI()

# Every route in students.py will start with /students.
app.include_router(students.router, prefix="/students")
```

In `routers/students.py`, you will need:

```python
from fastapi import APIRouter, HTTPException

# Create a router for student-related endpoints.
router = APIRouter()
```

To return one student:

```python
# Look up the student using the ID from the URL.
student = students.get(student_id)

if student is None:
    # If the ID is unknown, return a clear 404 error.
    raise HTTPException(status_code=404, detail="Student not found")

return student
```

#### Submission Checklist

Before you finish, make sure:

- Your virtual environment works.
- Your `requirements.txt` contains `fastapi` and `uvicorn`.
- `GET /` works in the browser.
- `GET /students` works in the browser.
- `GET /students/s1` works in the browser.
- `GET /students/s10` returns a `404` JSON error.
- You can explain what `/{student_id}` does.

Homework 1 is complete. Continue to [Homework 2](homework-2.md).
