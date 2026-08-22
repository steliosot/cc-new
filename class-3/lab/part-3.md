### Week 3 Part 3: Add Pydantic Validation

In Part 2, the API accepted a plain Python dictionary for new courses.

That works, but it is not safe enough. If the user forgets a field or sends the wrong type of data, our code may fail in a messy way.

In this part, you will add Pydantic schemas. A schema describes what data the API accepts and what data it returns.

#### What You Will Learn

- How Pydantic validates request bodies.
- How to use `Field` for simple rules.
- How to control the response shape.
- How FastAPI shows validation errors in `/docs`.

#### Part A: Create Schemas

1. Create `schemas.py`.

2. Add:

```python
from pydantic import BaseModel, ConfigDict, Field


# Data accepted when creating a course.
class CourseCreate(BaseModel):
    code: str = Field(min_length=2, max_length=20)
    title: str = Field(min_length=3, max_length=100)
    lecturer: str = Field(min_length=2, max_length=80)


# Data returned by the API.
class CourseResponse(BaseModel):
    id: int
    code: str
    title: str
    lecturer: str

    model_config = ConfigDict(from_attributes=True)
```

> [!TIP]
>
> Why does `CourseResponse` include `id`, but `CourseCreate` does not?
>
> <details>
> <summary>Show answer</summary>
>
> The client does not choose the `id`. The database creates it when the row is inserted.
>
> </details>

#### Part B: Update the App

3. Open `main.py`.

4. Add this import:

```python
import schemas
```

5. Replace the `/courses` routes with:

```python
@app.get("/courses", response_model=list[schemas.CourseResponse])
def get_courses(db: Session = Depends(get_db)):
    # Read all courses from the database.
    courses = db.query(models.Course).all()
    return courses


@app.get("/courses/{course_id}", response_model=schemas.CourseResponse)
def get_course(course_id: int, db: Session = Depends(get_db)):
    # Find one course by primary key.
    course = db.query(models.Course).filter(models.Course.id == course_id).first()

    if course is None:
        raise HTTPException(status_code=404, detail="Course not found")

    return course


@app.post("/courses", response_model=schemas.CourseResponse)
def create_course(course_data: schemas.CourseCreate, db: Session = Depends(get_db)):
    # course_data has already been checked by Pydantic.
    course = models.Course(
        code=course_data.code,
        title=course_data.title,
        lecturer=course_data.lecturer
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

#### Part C: Test Valid Data

6. Run:

```bash
uvicorn main:app --reload
```

7. Open:

```text
http://127.0.0.1:8000/docs
```

8. Test `POST /courses` with:

```json
{
  "code": "PY101",
  "title": "Python for APIs",
  "lecturer": "Nikos"
}
```

You should receive the new course with an `id`.

#### Part D: Test Invalid Data

9. Test `POST /courses` with:

```json
{
  "code": "X",
  "title": "No",
  "lecturer": "A"
}
```

FastAPI should reject the request before it reaches the database code.

<details>
<summary>Which part of the code rejects the short values?</summary>

```python
Field(min_length=...)
```

</details>

#### Part E: Test Everything in Postman

10. Open Postman.

11. Create a new request:

```text
GET http://127.0.0.1:8000/courses
```

Send the request and check that you receive the list of courses.

12. Create another request:

```text
GET http://127.0.0.1:8000/courses/1
```

Send the request and check that you receive one course.

13. Create a `POST` request:

```text
POST http://127.0.0.1:8000/courses
```

In Postman, go to **Body**, choose **raw**, choose **JSON**, and send:

```json
{
  "code": "PM101",
  "title": "Testing APIs with Postman",
  "lecturer": "Eleni"
}
```

14. Send another `POST` request with invalid data:

```json
{
  "code": "P",
  "title": "No",
  "lecturer": "A"
}
```

You should see a validation error.

15. Stop the server with `Ctrl + C`.

Part 3 is complete. Continue to [Homework 1](../homework/homework-1.md).
