# Week 3 SQL Quiz

## Question 1

What does a SQL table store?

- Rows of structured data
- Only Python functions
- Only image files
- GCP coupons

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: SQL tables store rows and columns of structured data.

## Question 2

What is SQLite in this lab?

- A SQL database stored in a local file
- A web browser
- A FastAPI router
- A password hashing library

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: SQLite stores the database in a file such as `app.db`.

## Question 3

What does SQLAlchemy help us do?

- Work with SQL databases using Python objects
- Create GCP projects
- Replace VS Code
- Run quizzes

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: SQLAlchemy maps Python classes to database tables and helps run queries.

## Question 4

What does `Base.metadata.create_all(bind=engine)` do?

- Creates database tables from models
- Deletes all records
- Starts Uvicorn
- Opens the browser

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: It creates tables based on the SQLAlchemy model definitions.

## Question 5

Why do we use `get_db()`?

- To open and close a database session for each request
- To create HTML pages
- To install FastAPI
- To list Linux users

Answer: 1
Imposters: 3
Type: single
Time: 35
Explanation: `get_db()` provides a database session and closes it after the request.

## Question 6

What does `response_model=schemas.FilmResponse` help with?

- It defines the shape of the API response
- It changes the VM password
- It creates a new database file
- It deletes a router

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: Response models help FastAPI validate and document response data.

## Question 7

What does `db.query(models.Film).all()` return?

- All film rows
- One user password
- The server port
- The requirements file

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: `.all()` returns all matching rows from the query.

## Question 8

Why do we use `HTTPException(status_code=404, detail="Film not found")`?

- To return a clear error when a film does not exist
- To create the films table
- To install SQLAlchemy
- To activate `.venv`

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: It returns a proper HTTP 404 JSON response.

## Question 9

What does CRUD stand for?

- Create, Read, Update, Delete
- Copy, Run, Upload, Download
- Connect, Route, Use, Deploy
- Code, Review, Undo, Debug

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: CRUD describes the four basic operations for stored data.

## Question 10

Why do we use FastAPI docs for `POST` requests?

- It lets us send a JSON request body
- It only shows static images
- It deletes the database automatically
- It replaces the terminal

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: FastAPI docs provides a form for sending request bodies and testing API methods.

