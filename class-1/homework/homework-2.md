### Week 1 Homework 2: Extend the University API

For homework, extend the University API from Homework 1.

Do not create a new project. Continue from:

```text
week1-university-api
```

#### Task 1: Add a Student Major Endpoint

Add this endpoint:

```text
GET /students/{student_id}/major
```

For this URL:

```text
http://127.0.0.1:8000/students/s1/major
```

the API should return:

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

<details>
<summary>What value does `student_id` have for `/students/s3/major`?</summary>

```text
s3
```

</details>

#### Task 2: Add a Courses Router

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

Remember to import and include the courses router in `main.py`.

#### Test

Run the API and test these URLs in the browser:

```text
http://127.0.0.1:8000/students/s1/major
http://127.0.0.1:8000/students/s10/major
http://127.0.0.1:8000/courses
http://127.0.0.1:8000/courses/c1
http://127.0.0.1:8000/courses/c10
```

#### What To Submit

Write a short post in the Teams collaboration space.

Include:

- one sentence explaining what you added
- one screenshot of one working endpoint
- one problem you had, or one thing you learned
