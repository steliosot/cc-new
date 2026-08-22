### Week 3 Part 2: Build a FastAPI App with SQLAlchemy

In this part, you will use the same database model inside a FastAPI app.

The important idea is this: FastAPI handles the web request, and SQLAlchemy handles the database work.

#### What You Will Learn

- How to install FastAPI in the same project.
- How to create a database dependency.
- How to return database rows from an API route.
- How to create a new database row from an API request.

#### Part A: Update the Requirements

1. Open `requirements.txt`.

2. Replace it with:

```text
fastapi
uvicorn
sqlalchemy
```

3. Install:

```bash
python -m pip install -r requirements.txt
```

#### Part B: Add a Database Dependency

4. Open `database.py`.

5. Add this function at the bottom:

```python
def get_db():
    # Open a database session for one API request.
    db = SessionLocal()
    try:
        yield db
    finally:
        # Always close the session after the request finishes.
        db.close()
```

FastAPI will use this function to give each route a database session.

> [!TIP]
>
> Why do we close the database session after each request?
>
> <details>
> <summary>Show answer</summary>
>
> Because the connection is no longer needed. Closing it avoids wasting database resources.
>
> </details>

#### Part C: Create the FastAPI App

6. Create `main.py`.

7. Add:

```python
from fastapi import Depends, FastAPI, HTTPException
from sqlalchemy.exc import IntegrityError
from sqlalchemy.orm import Session

import models
from database import engine, get_db

# Create the database tables when the app starts.
models.Base.metadata.create_all(bind=engine)

app = FastAPI()


@app.get("/")
def home():
    return {"message": "Week 3 SQLAlchemy API is running"}


@app.get("/courses")
def get_courses(db: Session = Depends(get_db)):
    # Read all courses from the database.
    courses = db.query(models.Course).all()
    return courses


@app.get("/courses/{course_id}")
def get_course(course_id: int, db: Session = Depends(get_db)):
    # Find one course by primary key.
    course = db.query(models.Course).filter(models.Course.id == course_id).first()

    if course is None:
        raise HTTPException(status_code=404, detail="Course not found")

    return course


@app.post("/courses")
def create_course(course_data: dict, db: Session = Depends(get_db)):
    # This first version accepts a plain dictionary.
    # In Part 3, we will replace it with Pydantic validation.
    course = models.Course(
        code=course_data["code"],
        title=course_data["title"],
        lecturer=course_data["lecturer"]
    )

    try:
        db.add(course)
        db.commit()
        db.refresh(course)
        return course

    except IntegrityError:
        db.rollback()
        raise HTTPException(status_code=400, detail="Course code already exists")
```

#### Part D: Run the API

8. Run:

```bash
uvicorn main:app --reload
```

9. Open the browser:

```text
http://127.0.0.1:8000/
```

You should see:

```json
{"message":"Week 3 SQLAlchemy API is running"}
```

10. Open:

```text
http://127.0.0.1:8000/courses
```

You should see the courses from Part 1.

11. Open:

```text
http://127.0.0.1:8000/docs
```

12. Test `POST /courses` with:

```json
{
  "code": "API101",
  "title": "API Development",
  "lecturer": "Alex"
}
```

<details>
<summary>What route should you open to see all courses in the browser?</summary>

```text
http://127.0.0.1:8000/courses
```

</details>

#### Part E: Stop and Think

13. Try to create another course with the same `code`.

You should get an error because `code` is unique.

14. Stop the server with `Ctrl + C`.

Part 2 is complete. Continue to [Part 3](part-3.md).
