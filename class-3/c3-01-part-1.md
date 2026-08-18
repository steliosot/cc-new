### Week 3 Part 1: SQL Database Setup

This tutorial introduces SQL databases with FastAPI.

For the first SQL lab, we will use SQLite because it is simple and runs from a local file. Later, we can move the same ideas to PostgreSQL.

#### What You Will Learn

- What a SQL database is.
- What SQLite is.
- How to create a FastAPI project with SQLAlchemy.
- How to create a database connection.
- How to define your first table with a model.

#### Part A: Create the Project

1. Open **Visual Studio Code**.

2. Create a new folder called:

```text
week3-sql-api
```

3. Open the folder in VS Code.

4. Open a VS Code terminal.

5. Create a virtual environment:

```bash
python3 -m venv .venv
```

On Windows, use:

```powershell
python -m venv .venv
```

6. Activate the virtual environment.

On macOS/Linux:

```bash
source .venv/bin/activate
```

On Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

7. Create a file called:

```text
requirements.txt
```

8. Add:

```text
fastapi
uvicorn
sqlalchemy
```

9. Install the packages:

```bash
python -m pip install -r requirements.txt
```

#### What Is SQL?

SQL databases store data in tables.

A table is like a spreadsheet:

- columns define the fields, such as `id`, `title`, and `year`
- rows store actual records
- a primary key identifies each row

#### What Is SQLite?

SQLite is a small SQL database stored in a file.

In this lab, the database file will be called:

```text
app.db
```

> [!TIP]
>
> Why is SQLite useful for a first SQL lab?
>
> <details>
> <summary>Show answer</summary>
>
> It does not need a separate database server. The database is just a file, so students can focus on SQL and FastAPI first.
>
> </details>

#### Part B: Create the Project Files

10. Create these files:

```text
main.py
database.py
models.py
```

Your project should look like this:

```text
week3-sql-api/
  .venv/
  database.py
  main.py
  models.py
  requirements.txt
```

11. Add this to `database.py`:

```python
# create_engine creates the connection to the database.
from sqlalchemy import create_engine

# declarative_base is used to create model classes.
# sessionmaker creates database sessions for queries.
from sqlalchemy.orm import declarative_base, sessionmaker

# This tells SQLAlchemy to use a SQLite database file called app.db.
DATABASE_URL = "sqlite:///./app.db"

engine = create_engine(
    DATABASE_URL,
    # SQLite needs this option when used with FastAPI.
    connect_args={"check_same_thread": False}
)

# SessionLocal creates database sessions.
# A session is used to talk to the database.
SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)

# Base is the parent class for our database models.
Base = declarative_base()
```

12. Add this to `models.py`:

```python
# These column types describe the fields in the SQL table.
from sqlalchemy import Column, Integer, String, Float

from database import Base


# Film is a SQLAlchemy model.
# It describes the films table in the database.
class Film(Base):
    __tablename__ = "films"

    # id is the primary key. It uniquely identifies each film.
    id = Column(Integer, primary_key=True, index=True)

    # nullable=False means this field is required.
    title = Column(String, nullable=False)
    year = Column(Integer, nullable=False)
    genre = Column(String, nullable=False)
    director = Column(String, nullable=False)
    rating = Column(Float, nullable=False)
```

#### What Is a Model?

A model is a Python class that describes a database table.

In this example:

- the table is called `films`
- each film has an `id`, `title`, `year`, `genre`, `director`, and `rating`
- `id` is the primary key

13. Add this to `main.py`:

```python
from fastapi import FastAPI

import models
from database import engine

# Create database tables from the SQLAlchemy models.
# If the tables already exist, SQLAlchemy leaves them in place.
models.Base.metadata.create_all(bind=engine)

app = FastAPI()


@app.get("/")
def home():
    # Simple test endpoint so we know the API is running.
    return {"message": "Week 3 SQL API is running"}
```

14. Run the app:

```bash
uvicorn main:app --reload
```

15. Open your browser:

```text
http://127.0.0.1:8000/
```

You should see:

```json
{"message":"Week 3 SQL API is running"}
```

16. Stop the server with `Ctrl + C`.

17. Check your project folder. You should now see:

```text
app.db
```

This is your SQLite database file.

Part 1 is complete. Continue to [Part 2](c3-02-part-2.md).
