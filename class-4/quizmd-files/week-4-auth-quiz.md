# Week 4 Validation and Authentication Quiz

## Question 1

What does validation do?

- Checks incoming data before the API uses it
- Deletes the database
- Starts the VM
- Creates a GCP coupon

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: Validation checks whether input data has the right shape and values.

## Question 2

What does `EmailStr` validate?

- That a value looks like an email address
- That a password is hashed
- That a route is protected
- That SQLite is installed

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: `EmailStr` validates email-like strings.

## Question 3

Why should passwords be hashed?

- To avoid storing plain text passwords
- To make the API slower
- To create a database table
- To replace JWT

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: Hashed passwords are safer if the database is exposed.

## Question 4

What does the register endpoint create?

- A new user
- A new VM
- A new quiz
- A new browser tab

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: Registration creates a user record with a hashed password.

## Question 5

What does the login endpoint return after a successful login?

- An access token
- The plain text password
- The whole database
- The VM external IP

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: Login returns a token that can be used for protected routes.

## Question 6

What is a JWT used for in this lab?

- Proving that a client has logged in
- Creating SQL tables
- Installing Python packages
- Running Apache

Answer: 1
Imposters: 3
Type: single
Time: 35
Explanation: A JWT contains signed data that the API can verify.

## Question 7

What does `Depends(get_current_user)` do?

- Requires a valid logged-in user for the route
- Installs Uvicorn
- Creates a password hash
- Deletes invalid users

Answer: 1
Imposters: 3
Type: single
Time: 35
Explanation: It runs the dependency and gives the route the current authenticated user.

## Question 8

What should happen when `/profile/me` is called without a token?

- The API should reject the request
- The API should create a new user
- The API should return all passwords
- The API should delete the profile router

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: Protected routes should not work without authentication.

## Question 9

Why should the lab `SECRET_KEY` not be hard-coded in real applications?

- Secrets in source code can leak
- FastAPI cannot read strings
- SQLite cannot store users
- Routers require numbers only

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: Real secrets should be stored outside source code, such as in environment variables.

## Question 10

What does the FastAPI docs **Authorize** button help you test?

- Protected endpoints that require a token
- Only static HTML files
- Linux user deletion
- GCP coupon redemption

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: The Authorize button lets you send the token with requests from the docs interface.

