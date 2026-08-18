### Week 3 Part 2: Read Data from SQL

In this part, you will insert film data into SQLite and read it through FastAPI endpoints.

#### What You Will Learn

- How to create a database session.
- How to insert starter data.
- How to create API routes backed by SQL.
- How to return all rows.
- How to return one row by ID.
- How to return a `404` error.

#### Part A: Add Database Session Dependency

1. Open `database.py`.

2. Add this function at the bottom:

```python
def get_db():
    # Open a database session for one request.
    db = SessionLocal()
    try:
        yield db
    finally:
        # Always close the session when the request is finished.
        db.close()
```

FastAPI will use this function to open and close a database session for each request.

> [!TIP]
>
> Why should the database session be closed after the request finishes?
>
> <details>
> <summary>Show answer</summary>
>
> It releases database resources. Leaving sessions open can cause bugs and resource problems as the app grows.
>
> </details>

#### Part B: Create Schemas

3. Create a new file called:

```text
schemas.py
```

4. Add:

```python
from pydantic import BaseModel


# This schema controls how film data is returned by the API.
class FilmResponse(BaseModel):
    id: int
    title: str
    year: int
    genre: str
    director: str
    rating: float

    class Config:
        # Allows Pydantic to read data from SQLAlchemy model objects.
        from_attributes = True
```

Schemas describe the shape of data sent by the API.

#### Part C: Seed the Database

5. Create a file called:

```text
seed.py
```

6. Add:

```python
from database import SessionLocal
from models import Film

# Open a database session.
db = SessionLocal()

# Create Python objects that match the films table.
films = [
    Film(title="The Hobbit", year=2012, genre="Fantasy", director="Peter Jackson", rating=7.8),
    Film(title="The Matrix", year=1999, genre="Science Fiction", director="The Wachowskis", rating=8.7),
    Film(title="Spirited Away", year=2001, genre="Animation", director="Hayao Miyazaki", rating=8.6),
]

for film in films:
    # Stage each film to be inserted into the database.
    db.add(film)

# Save all staged changes.
db.commit()

# Close the database session.
db.close()

print("Database seeded")
```

7. Run the seed script once:

```bash
python seed.py
```

You should see:

```text
Database seeded
```

#### Part D: Create a Films Router

8. Create a folder called:

```text
routers
```

9. Inside `routers`, create an empty file:

```text
__init__.py
```

10. Inside `routers`, create:

```text
films.py
```

11. Add this code to `routers/films.py`:

```python
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session

import models
import schemas
from database import get_db

router = APIRouter()


@router.get("/", response_model=list[schemas.FilmResponse])
def get_films(db: Session = Depends(get_db)):
    # Query the films table and return every row.
    return db.query(models.Film).all()


@router.get("/{film_id}", response_model=schemas.FilmResponse)
def get_film(film_id: int, db: Session = Depends(get_db)):
    # Find the first film where the id column matches the URL value.
    film = db.query(models.Film).filter(models.Film.id == film_id).first()

    if film is None:
        # Return a 404 error instead of returning empty data.
        raise HTTPException(status_code=404, detail="Film not found")

    return film
```

#### Part E: Connect the Router

12. Open `main.py`.

13. Replace it with:

```python
from fastapi import FastAPI

import models
from database import engine
from routers import films

# Create tables before the app starts serving requests.
models.Base.metadata.create_all(bind=engine)

app = FastAPI()

# Connect the films router under /films.
app.include_router(films.router, prefix="/films")


@app.get("/")
def home():
    return {"message": "Week 3 SQL API is running"}
```

14. Run the server:

```bash
uvicorn main:app --reload
```

15. Open your browser:

```text
http://127.0.0.1:8000/films
```

You should see all films.

16. Open:

```text
http://127.0.0.1:8000/films/1
```

You should see one film.

17. Open:

```text
http://127.0.0.1:8000/films/999
```

You should see:

```json
{"detail":"Film not found"}
```

> [!TIP]
>
> What type is `film_id` in `get_film(film_id: int, ...)`, and why is that useful?
>
> <details>
> <summary>Show answer</summary>
>
> It is an integer. FastAPI converts the path value and rejects invalid values before the query runs.
>
> </details>

Part 2 is complete. Continue to [Part 3](c3-03-part-3.md).
