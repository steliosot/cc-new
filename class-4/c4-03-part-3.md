### Week 4 Part 3: Protect API Routes with JWT

In this part, you will protect an API route using the JWT token from login.

#### What You Will Learn

- What a bearer token is.
- How to read the current user from a token.
- How to protect a FastAPI route.
- How to test protected routes in `/docs`.

#### Part A: Add Token Reading

1. Open `security.py`.

2. Add these imports:

```python
from fastapi import Depends, HTTPException
from fastapi.security import HTTPAuthorizationCredentials, HTTPBearer
from jose import JWTError
from sqlalchemy.orm import Session

import models
from database import get_db
```

3. Add this under the constants:

```python
# HTTPBearer tells FastAPI to expect an Authorization: Bearer <token> header.
bearer_scheme = HTTPBearer()
```

4. Add this function at the bottom:

```python
def get_current_user(
    credentials: HTTPAuthorizationCredentials = Depends(bearer_scheme),
    db: Session = Depends(get_db)
):
    # Extract the token value from the Authorization header.
    token = credentials.credentials

    try:
        # Decode and verify the JWT.
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])

        # We stored the user's email in the token subject.
        email = payload.get("sub")

        if email is None:
            raise HTTPException(status_code=401, detail="Invalid token")

    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")

    # Find the user represented by the token.
    user = db.query(models.User).filter(models.User.email == email).first()

    if user is None:
        raise HTTPException(status_code=401, detail="User not found")

    return user
```

#### Part B: Create a Protected Router

5. Create `routers/profile.py`:

```python
from fastapi import APIRouter, Depends

import models
from security import get_current_user

router = APIRouter()


@router.get("/me")
def get_profile(current_user: models.User = Depends(get_current_user)):
    # This function only runs if get_current_user returns a valid user.
    return {
        "id": current_user.id,
        "username": current_user.username,
        "email": current_user.email
    }
```

6. Open `main.py`.

7. Update the router import:

```python
from routers import auth, profile
```

8. Add:

```python
app.include_router(profile.router, prefix="/profile")
```

#### Part C: Test Without a Token

9. Run the app:

```bash
uvicorn main:app --reload
```

10. Open:

```text
http://127.0.0.1:8000/profile/me
```

You should get an authentication error because you did not send a token.

The response should include:

```json
{"detail":"Not authenticated"}
```

#### Part D: Test With a Token in Docs

11. Open:

```text
http://127.0.0.1:8000/docs
```

12. Use `POST /auth/login` to log in.

13. Copy the `access_token` value.

14. Click the **Authorize** button at the top of the docs page.

15. Paste the token.

16. Click **Authorize**.

17. Test `GET /profile/me`.

You should see the logged-in user's profile.

> [!TIP]
>
> What does a bearer token prove to the API?
>
> <details>
> <summary>Show answer</summary>
>
> It proves that the client has a valid token created by the API. The API can decode it and identify the user.
>
> </details>

#### Completion Checklist

Before you finish, make sure:

- invalid registration data is rejected
- duplicate email registration is rejected
- login returns an access token
- `/profile/me` fails without a token
- `/profile/me` works with a valid token
- you can explain why passwords should be hashed

Part 3 is complete. Now complete [Quiz 1](c4-04-quiz-1.md).
