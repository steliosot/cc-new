### Week 2 Homework 1: Simple CRUD with FastAPI and SQLite

In this homework exercise, you will build a simple CRUD API using FastAPI and SQLite.

CRUD means:

- Create
- Read
- Update
- Delete

Most database-backed applications use these four actions. For example, a notes app needs to create a note, read notes, edit a note, and delete a note.

We will use a small `notes` table to keep the example simple.

#### What You Will Learn

- How to create rows from an API request.
- How to read rows from SQLite.
- How to update rows.
- How to delete rows.
- How to test non-GET endpoints using FastAPI docs.

#### Part A: Create a New Project

1. Create a new folder called:

```text
week2-notes-crud
```

2. Open it in VS Code.

3. Open a new VS Code terminal.

4. Create and activate a virtual environment.

5. Create `requirements.txt`:

```text
fastapi
uvicorn
```

6. Install the requirements.

7. Create `main.py`.

#### Part B: Add the CRUD API

8. Add this code to `main.py`:

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import sqlite3

app = FastAPI()


class NoteCreate(BaseModel):
    title: str
    text: str


class NoteUpdate(BaseModel):
    title: str | None = None
    text: str | None = None


def get_connection():
    connection = sqlite3.connect("notes.db")
    connection.row_factory = sqlite3.Row
    return connection


def create_table():
    connection = get_connection()
    cursor = connection.cursor()
    cursor.execute(
        """
        CREATE TABLE IF NOT EXISTS notes (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            title TEXT NOT NULL,
            text TEXT NOT NULL
        )
        """
    )
    connection.commit()
    connection.close()


create_table()


@app.get("/")
def home():
    return {"message": "Notes API is running"}


@app.get("/notes")
def get_notes():
    connection = get_connection()
    cursor = connection.cursor()
    cursor.execute("SELECT * FROM notes")
    rows = cursor.fetchall()
    connection.close()
    return [dict(row) for row in rows]


@app.get("/notes/{note_id}")
def get_note(note_id: int):
    connection = get_connection()
    cursor = connection.cursor()
    cursor.execute("SELECT * FROM notes WHERE id = ?", (note_id,))
    row = cursor.fetchone()
    connection.close()

    if row is None:
        raise HTTPException(status_code=404, detail="Note not found")

    return dict(row)


@app.post("/notes")
def create_note(note: NoteCreate):
    connection = get_connection()
    cursor = connection.cursor()
    cursor.execute(
        "INSERT INTO notes (title, text) VALUES (?, ?)",
        (note.title, note.text)
    )
    connection.commit()
    note_id = cursor.lastrowid
    connection.close()
    return {"id": note_id, "title": note.title, "text": note.text}


@app.patch("/notes/{note_id}")
def update_note(note_id: int, note: NoteUpdate):
    connection = get_connection()
    cursor = connection.cursor()

    cursor.execute("SELECT * FROM notes WHERE id = ?", (note_id,))
    existing = cursor.fetchone()

    if existing is None:
        connection.close()
        raise HTTPException(status_code=404, detail="Note not found")

    new_title = note.title if note.title is not None else existing["title"]
    new_text = note.text if note.text is not None else existing["text"]

    cursor.execute(
        "UPDATE notes SET title = ?, text = ? WHERE id = ?",
        (new_title, new_text, note_id)
    )
    connection.commit()
    connection.close()

    return {"id": note_id, "title": new_title, "text": new_text}


@app.delete("/notes/{note_id}")
def delete_note(note_id: int):
    connection = get_connection()
    cursor = connection.cursor()

    cursor.execute("SELECT * FROM notes WHERE id = ?", (note_id,))
    existing = cursor.fetchone()

    if existing is None:
        connection.close()
        raise HTTPException(status_code=404, detail="Note not found")

    cursor.execute("DELETE FROM notes WHERE id = ?", (note_id,))
    connection.commit()
    connection.close()

    return {"message": "Note deleted"}
```

> [!TIP]
>
> Why do we use `CREATE TABLE IF NOT EXISTS`?
>
> <details>
> <summary>Show answer</summary>
>
> It creates the table the first time the app runs, but avoids an error if the table already exists.
>
> </details>

> [!TIP]
>
> Why do we use `?` placeholders in SQL commands such as `WHERE id = ?`?
>
> <details>
> <summary>Show answer</summary>
>
> Placeholders keep user input separate from the SQL command. This is safer and helps avoid SQL injection mistakes.
>
> </details>

#### Part C: Run and Test

9. Run the app:

```bash
uvicorn main:app --reload
```

10. Open FastAPI docs:

```text
http://127.0.0.1:8000/docs
```

11. Test `POST /notes` with:

```json
{
  "title": "First note",
  "text": "This note is stored in SQLite."
}
```

12. Open in the browser:

```text
http://127.0.0.1:8000/notes
```

You should see your note.

13. In `/docs`, test:

- `GET /notes/{note_id}`
- `PATCH /notes/{note_id}`
- `DELETE /notes/{note_id}`

> [!TIP]
>
> Why do we use `/docs` for `POST`, `PATCH`, and `DELETE`?
>
> <details>
> <summary>Show answer</summary>
>
> The browser address bar is mainly for GET requests. FastAPI docs lets us send request bodies and choose methods such as POST, PATCH, and DELETE.
>
> </details>

#### Checklist

Before you finish, make sure:

- `GET /notes` returns notes
- `GET /notes/{note_id}` returns one note
- `POST /notes` creates a note
- `PATCH /notes/{note_id}` updates a note
- `DELETE /notes/{note_id}` deletes a note
- missing notes return a `404` error

Homework 1 is complete. Continue to [Homework 2](homework-2.md).
