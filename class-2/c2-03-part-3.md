### Week 2 Part 3: University API Exercise

Now build a small University API using FastAPI.

Create a new project in VS Code. Do not reuse the movie project from Part 2.

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
week2-university-api
```

2. Open it in VS Code.

3. Open a VS Code terminal.

4. Create and activate a virtual environment.

5. Create a `requirements.txt` file with:

```text
fastapi
uvicorn
```

6. Install the requirements:

```bash
python -m pip install -r requirements.txt
```

#### Part B: Create the Files

Create this structure:

```text
week2-university-api/
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

```bash
uvicorn main:app --reload
```

Open these URLs in your browser:

```text
http://127.0.0.1:8000/
```

```text
http://127.0.0.1:8000/students
```

```text
http://127.0.0.1:8000/students/s1
```

```text
http://127.0.0.1:8000/students/s10
```

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

#### Part F: Add One More Endpoint

Add one extra endpoint:

```text
GET /students/s2/major
```

It should return only Maria Garcia's major:

```json
{
  "student_id": "s2",
  "major": "Mathematics"
}
```

Then make it work for any student ID:

```text
GET /students/{student_id}/major
```

For example:

```text
http://127.0.0.1:8000/students/s1/major
```

should return:

```json
{
  "student_id": "s1",
  "major": "Computer Science"
}
```

If the student does not exist, return:

```json
{"detail":"Student not found"}
```

> [!TIP]
>
> This endpoint has a fixed part and a variable part: `/students/{student_id}/major`. What value does `student_id` have for `/students/s3/major`?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> s3
> ```
>
> </details>

#### Optional Challenge

Add a second router called `courses`.

Create this file:

```text
routers/courses.py
```

Add these endpoints:

- `GET /courses` returns all courses.
- `GET /courses/{course_id}` returns one course.
- unknown course IDs return a `404` error.

Use this data:

```python
courses = {
    "c1": {
        "title": "Cloud Computing",
        "level": "Year 2",
        "credits": 15
    },
    "c2": {
        "title": "Databases",
        "level": "Year 2",
        "credits": 15
    }
}
```

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

If you complete the optional challenge, you will also need to import and include the courses router.

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
- `GET /students/s1/major` works in the browser.
- `GET /students/s10` returns a `404` JSON error.
- `GET /students/s10/major` returns a `404` JSON error.
- You can explain what `/{student_id}` does.
- Optional: your courses router works.
