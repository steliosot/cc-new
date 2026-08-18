# Week 2 FastAPI Quiz

## Question 1

What is FastAPI used for in this lab?

- Building APIs with Python
- Installing Ubuntu packages
- Creating GCP projects
- Running Linux user-management commands

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: FastAPI is the Python framework used to create API endpoints.

## Question 2

What does `uvicorn main:app --reload` do?

- Runs the FastAPI app from `main.py`
- Creates a virtual environment
- Installs packages from `requirements.txt`
- Deletes the API server

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: `uvicorn` runs the app object named `app` inside `main.py`. `--reload` restarts the server when code changes.

## Question 3

What is the purpose of `requirements.txt`?

- It lists the Python packages needed by the project
- It stores API responses
- It contains the VM external IP address
- It replaces `main.py`

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: `requirements.txt` helps everyone install the same project dependencies.

## Question 4

Which command installs packages from `requirements.txt`?

- `python -m pip install -r requirements.txt`
- `uvicorn main:app --reload`
- `python3 -m venv .venv`
- `source .venv/bin/activate`

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: `pip install -r requirements.txt` installs all packages listed in the requirements file.

## Question 5

What does `@app.get("/")` create?

- A GET endpoint for the root URL
- A new Python package
- A virtual environment
- A folder called `/`

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: The decorator connects the function below it to the GET `/` endpoint.

## Question 6

What URL opens the local FastAPI app on port `8000`?

- `http://127.0.0.1:8000/`
- `http://127.0.0.1:3000/`
- `https://gcp.local/`
- `http://fastapi/main.py`

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: Uvicorn uses port `8000` by default, and `127.0.0.1` means the local machine.

## Question 7

Why do we use `APIRouter`?

- To organise related endpoints in separate files
- To install FastAPI
- To start VS Code
- To create a GCP VM

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: `APIRouter` keeps routes organised, such as putting movie routes in `routers/movies.py`.

## Question 8

If `main.py` uses `prefix="/movies"` and the router has `@router.get("/{movie_id}")`, what URL gets the Hobbit movie?

- `/movies/hobbit`
- `/hobbit/movies`
- `/movie_id/hobbit`
- `/movies`

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: The prefix `/movies` is added before the router path `/{movie_id}`.

## Question 9

What does `{student_id}` mean in `/students/{student_id}`?

- It captures a changing value from the URL
- It creates a new student automatically
- It means the route only works for `student_id`
- It installs the students router

Answer: 1
Imposters: 3
Type: single
Time: 35
Explanation: FastAPI passes the captured URL value into the function parameter.

## Question 10

Why do we use `HTTPException(status_code=404, detail="Student not found")`?

- To return a clear JSON error when a student does not exist
- To stop the Uvicorn server
- To create the `students` dictionary
- To install `fastapi`

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: `HTTPException` lets the API return a proper HTTP status code and message.

