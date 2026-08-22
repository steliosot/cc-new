### Week 3 Homework 1: SQLAlchemy Exercise

In this exercise, you will create a new SQLAlchemy API from zero.

Use Part 1, Part 2, and Part 3 if you struggle.

#### Task

Create a FastAPI application for books.

Your project folder should be called:

```text
week3-books-api
```

#### Requirements

Create these files:

```text
requirements.txt
database.py
models.py
schemas.py
main.py
```

Your `requirements.txt` should contain:

```text
fastapi
uvicorn
sqlalchemy
```

#### Database

Create a SQLite database file called:

```text
books.db
```

Create a SQLAlchemy model called `Book`.

The table should be called:

```text
books
```

The table should have:

- `id`
- `title`
- `author`
- `year`

#### Validation

Create a Pydantic schema called `BookCreate`.

Use validation rules:

- `title` must have at least 2 characters.
- `author` must have at least 2 characters.
- `year` must be greater than or equal to 1900.

<details>
<summary>Which Pydantic helper can you use for these rules?</summary>

```python
Field(...)
```

</details>

#### API Routes

Create these routes:

```text
GET /
GET /books
GET /books/{book_id}
POST /books
```

The home route should return:

```json
{"message":"Books API is running"}
```

#### Test

Run the app with Uvicorn.

<details>
<summary>Do you remember the command?</summary>

```bash
uvicorn main:app --reload
```

</details>

First open:

```text
http://127.0.0.1:8000/docs
```

Test:

- create a valid book
- create an invalid book with a short title
- get all books
- get one book by id
- try to get a book id that does not exist

Then test everything again in Postman.

In Postman, create requests for:

```text
GET http://127.0.0.1:8000/books
GET http://127.0.0.1:8000/books/1
POST http://127.0.0.1:8000/books
```

For the `POST` request, use **Body**, then **raw**, then **JSON**.

Send valid data:

```json
{
  "title": "Clean Code",
  "author": "Robert Martin",
  "year": 2008
}
```

Also send invalid data to check that validation works:

```json
{
  "title": "A",
  "author": "B",
  "year": 1800
}
```

#### Checklist

Before you finish, make sure:

- the app starts without errors
- SQLAlchemy creates the `books` table
- valid books can be inserted
- invalid data is rejected
- missing books return `404`
- all routes were tested in Postman
- you can explain the difference between a SQLAlchemy model and a Pydantic schema

Week 3 is complete.
