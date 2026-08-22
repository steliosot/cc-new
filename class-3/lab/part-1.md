### Week 3 Part 1: Create a Local SQLite Database with SQLAlchemy

In Week 2, you used SQLite directly. In this part, you will use SQLAlchemy to create and use a SQLite database from Python.

SQLAlchemy is a Python library that helps us work with databases. Instead of writing SQL commands everywhere, we can create Python classes that represent database tables.

#### What You Will Learn

- What an ORM is.
- How a SQLAlchemy model represents a table.
- How to create a local SQLite database.
- How to insert and read data using a session.

#### Part A: Open a New Project Folder

1. Close your previous project in VS Code.

2. Create a new folder:

```text
week3-sqlalchemy-api
```

3. Open this folder in VS Code.

4. Create and activate a virtual environment.

<details>
<summary>Do you remember the command to create a virtual environment?</summary>

```bash
python -m venv .venv
```

</details>

<details>
<summary>Do you remember the command to activate it on macOS or Linux?</summary>

```bash
source .venv/bin/activate
```

</details>

5. Create `requirements.txt`:

```text
sqlalchemy
```

6. Install the requirements:

```bash
python -m pip install -r requirements.txt
```

#### Part B: Create the Database Code

7. Create `database.py`.

8. Add:

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import declarative_base, sessionmaker

# This creates a SQLite database file called courses.db.
DATABASE_URL = "sqlite:///./courses.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)

# SessionLocal creates database sessions.
# A session is the object we use to talk to the database.
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

# Base is used by our models.
Base = declarative_base()
```

> [!TIP]
>
> What file will be created by this database connection?
>
> <details>
> <summary>Show answer</summary>
>
> A local SQLite file called `courses.db`.
>
> </details>

#### Part C: Create a Model

9. Create `models.py`.

10. Add:

```python
from sqlalchemy import Column, Integer, String

from database import Base


# Course is a Python class, but SQLAlchemy maps it to the courses table.
class Course(Base):
    __tablename__ = "courses"

    id = Column(Integer, primary_key=True, index=True)
    code = Column(String, unique=True, nullable=False)
    title = Column(String, nullable=False)
    lecturer = Column(String, nullable=False)
```

In this model:

- `__tablename__` is the table name.
- `id` is the primary key.
- `code`, `title`, and `lecturer` are text columns.
- `nullable=False` means the value is required.
- `unique=True` means the course code cannot be repeated.

<details>
<summary>What is the table name created by this model?</summary>

```text
courses
```

</details>

#### Part D: Create and Seed the Database

11. Create `seed.py`.

12. Add:

```python
import models
from database import SessionLocal, engine

# Create all tables described by the models.
models.Base.metadata.create_all(bind=engine)

# Open a database session.
db = SessionLocal()

try:
    courses = [
        models.Course(code="CC101", title="Cloud Computing", lecturer="Stelios"),
        models.Course(code="DB101", title="Databases", lecturer="Maria"),
    ]

    for course in courses:
        existing_course = (
            db.query(models.Course)
            .filter(models.Course.code == course.code)
            .first()
        )

        if existing_course is None:
            db.add(course)

    db.commit()

    print("Database created and courses checked.")

finally:
    db.close()
```

13. Run:

```bash
python seed.py
```

You should see:

```text
Database created and courses checked.
```

#### Part E: Read the Data

14. Create `read_courses.py`.

15. Add:

```python
import models
from database import SessionLocal

db = SessionLocal()

try:
    courses = db.query(models.Course).all()

    for course in courses:
        print(course.id, course.code, course.title, course.lecturer)

finally:
    db.close()
```

16. Run:

```bash
python read_courses.py
```

You should see the courses printed in the terminal.

> [!TIP]
>
> What does `db.query(models.Course).all()` do?
>
> <details>
> <summary>Show answer</summary>
>
> It asks SQLAlchemy to select all rows from the `courses` table and return them as `Course` objects.
>
> </details>

#### Part F: Stop and Check

17. Make sure your folder now contains:

```text
database.py
models.py
seed.py
read_courses.py
requirements.txt
courses.db
```

Part 1 is complete. Continue to [Part 2](part-2.md).
