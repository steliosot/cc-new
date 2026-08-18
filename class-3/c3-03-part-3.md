### Week 3 Part 3: MiniPost CRUD API

In this part, you will build a small CRUD API called MiniPost.

CRUD means:

- Create
- Read
- Update
- Delete

#### What You Will Learn

- How to create a second SQL table.
- How to create request schemas.
- How to use FastAPI docs for `POST`, `PATCH`, and `DELETE`.
- How to build a full CRUD API.

#### Part A: Add the Post Model

1. Stop the server with `Ctrl + C`.

2. Open `models.py`.

3. Add this class under the `Film` class:

```python
class Post(Base):
    __tablename__ = "posts"

    # Every post gets an auto-generated integer ID.
    id = Column(Integer, primary_key=True, index=True)

    # These fields store the post content.
    user = Column(String, nullable=False)
    title = Column(String, nullable=False)
    text = Column(String, nullable=False)
    hashtag = Column(String, nullable=False)
    location = Column(String, nullable=False)
    url = Column(String, nullable=False)
```

#### Part B: Add Post Schemas

4. Open `schemas.py`.

5. Add:

```python
# Data required when creating a post.
class PostCreate(BaseModel):
    user: str
    title: str
    text: str
    hashtag: str
    location: str
    url: str


# Data allowed when updating a post.
# Each field is optional, so students can update only one field.
class PostUpdate(BaseModel):
    user: str | None = None
    title: str | None = None
    text: str | None = None
    hashtag: str | None = None
    location: str | None = None
    url: str | None = None


# Data returned by the API after reading, creating, or updating a post.
class PostResponse(BaseModel):
    id: int
    user: str
    title: str
    text: str
    hashtag: str
    location: str
    url: str

    class Config:
        from_attributes = True
```

#### Part C: Create the Posts Router

6. Create a file:

```text
routers/posts.py
```

7. Add:

```python
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session

import models
import schemas
from database import get_db

router = APIRouter()


@router.post("/", response_model=schemas.PostResponse)
def create_post(post: schemas.PostCreate, db: Session = Depends(get_db)):
    # Convert the validated request body into a SQLAlchemy Post object.
    db_post = models.Post(**post.model_dump())

    # Add, save, and refresh so the new ID is available.
    db.add(db_post)
    db.commit()
    db.refresh(db_post)
    return db_post


@router.get("/", response_model=list[schemas.PostResponse])
def get_posts(db: Session = Depends(get_db)):
    # Return every post in the database.
    return db.query(models.Post).all()


@router.get("/{post_id}", response_model=schemas.PostResponse)
def get_post(post_id: int, db: Session = Depends(get_db)):
    # Find one post by its ID.
    post = db.query(models.Post).filter(models.Post.id == post_id).first()

    if post is None:
        raise HTTPException(status_code=404, detail="Post not found")

    return post


@router.patch("/{post_id}", response_model=schemas.PostResponse)
def update_post(post_id: int, post_update: schemas.PostUpdate, db: Session = Depends(get_db)):
    # First find the existing post.
    post = db.query(models.Post).filter(models.Post.id == post_id).first()

    if post is None:
        raise HTTPException(status_code=404, detail="Post not found")

    # Only include fields the client actually sent.
    update_data = post_update.model_dump(exclude_unset=True)

    for key, value in update_data.items():
        # Update each provided field on the post object.
        setattr(post, key, value)

    db.commit()
    db.refresh(post)
    return post


@router.delete("/{post_id}")
def delete_post(post_id: int, db: Session = Depends(get_db)):
    # Find the post before deleting it.
    post = db.query(models.Post).filter(models.Post.id == post_id).first()

    if post is None:
        raise HTTPException(status_code=404, detail="Post not found")

    db.delete(post)
    db.commit()
    return {"message": "Post deleted"}
```

#### Part D: Connect the Posts Router

8. Open `main.py`.

9. Update the imports:

```python
from routers import films, posts
```

10. Add this under the films router:

```python
app.include_router(posts.router, prefix="/posts")
```

11. Run the server:

```bash
uvicorn main:app --reload
```

#### Part E: Test in FastAPI Docs

12. Open:

```text
http://127.0.0.1:8000/docs
```

13. Find `POST /posts/`.

14. Click **Try it out**.

15. Use this JSON:

```json
{
  "user": "Stelios",
  "title": "My first post",
  "text": "This is my first SQL-backed API post.",
  "hashtag": "#cloud",
  "location": "London",
  "url": "https://example.com"
}
```

16. Click **Execute**.

17. Test:

```text
http://127.0.0.1:8000/posts
```

18. Test:

```text
http://127.0.0.1:8000/posts/1
```

19. Go back to `/docs` and test:

- `PATCH /posts/{post_id}`
- `DELETE /posts/{post_id}`

> [!TIP]
>
> Why do we use `/docs` for `POST`, `PATCH`, and `DELETE` instead of only the browser address bar?
>
> <details>
> <summary>Show answer</summary>
>
> The browser address bar is mainly for GET requests. FastAPI docs lets us send request bodies and choose methods such as POST, PATCH, and DELETE.
>
> </details>

#### Completion Checklist

Before you finish, make sure:

- `GET /films` works.
- `GET /films/1` works.
- `GET /films/999` returns a 404.
- `POST /posts/` creates a post in `/docs`.
- `GET /posts` returns posts.
- `GET /posts/1` returns one post.
- `PATCH /posts/1` updates a post.
- `DELETE /posts/1` deletes a post.

Part 3 is complete. Now complete [Quiz 1](c3-04-quiz-1.md).
