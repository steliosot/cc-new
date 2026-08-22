### Week 4 Part 1: Validation with Pydantic

This tutorial introduces validation in FastAPI.

Validation means checking that incoming data has the right shape before the API uses it.

#### What You Will Learn

- How Pydantic validates request bodies.
- How to use field constraints.
- How FastAPI returns validation errors.
- How to test invalid data in `/docs`.

#### Part A: Create the Project

1. Create a new folder:

```text
week4-auth-api
```

2. Open it in VS Code.

3. Create and activate a virtual environment.

4. Create `requirements.txt`:

```text
fastapi
uvicorn
sqlalchemy
passlib[bcrypt]
bcrypt<5
python-jose[cryptography]
```

5. Install:

```bash
python -m pip install -r requirements.txt
```

#### Part B: Create a Small Validation App

6. Create `main.py`.

7. Add:

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr, Field

app = FastAPI()


# This schema describes the data accepted by /register-preview.
# FastAPI checks the rules before the function runs.
class UserRegister(BaseModel):
    # username must be between 3 and 50 characters.
    username: str = Field(min_length=3, max_length=50)

    # EmailStr checks that the value looks like an email address.
    email: EmailStr

    # password must be at least 6 characters.
    password: str = Field(min_length=6)


@app.post("/register-preview")
def register_preview(user: UserRegister):
    # If we reach this point, validation has passed.
    # Do not return the password in the response.
    return {
        "username": user.username,
        "email": user.email,
        "message": "Validation passed"
    }
```

8. Add `email-validator` to `requirements.txt`:

```text
email-validator
```

9. Install again:

```bash
python -m pip install -r requirements.txt
```

10. Run:

```bash
uvicorn main:app --reload
```

11. Open:

```text
http://127.0.0.1:8000/docs
```

12. Test `POST /register-preview` with:

```json
{
  "username": "stelios",
  "email": "stelios@example.com",
  "password": "secret123"
}
```

13. Now try invalid data:

```json
{
  "username": "st",
  "email": "not-an-email",
  "password": "123"
}
```

FastAPI should return validation errors.

> [!TIP]
>
> Why is it useful for FastAPI to reject bad data before it reaches our database code?
>
> <details>
> <summary>Show answer</summary>
>
> It prevents invalid or incomplete data from entering the application. This makes the API safer and easier to debug.
>
> </details>

#### Part C: Stop the Server

14. Stop the server with `Ctrl + C`.

Part 1 is complete. Continue to [Part 2](part-2.md).
