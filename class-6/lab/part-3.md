### Week 6 Part 3: Docker Exercise

In this exercise, you will containerize a small FastAPI API from zero.

Use Part 1 and Part 2 if you struggle.

#### Task

Create a new project folder:

```text
week6-courses-container
```

Create these files:

```text
requirements.txt
main.py
Dockerfile
```

#### API Requirements

Your FastAPI app should have these routes:

```text
GET /
GET /courses
GET /courses/{course_id}
```

Use this data:

```python
courses = {
    "c1": {
        "title": "Cloud Computing",
        "level": "Year 2"
    },
    "c2": {
        "title": "Databases",
        "level": "Year 2"
    }
}
```

If a course does not exist, return:

```json
{"detail":"Course not found"}
```

with status code `404`.

<details>
<summary>Which FastAPI class can return a `404` error?</summary>

```python
HTTPException
```

</details>

#### Docker Requirements

Your Docker image should be called:

```text
week6-courses-api:1
```

Your container should be called:

```text
courses-api
```

Run the container so the API is available on VM port `80`.

<details>
<summary>Which Docker option maps a VM port to a container port?</summary>

```bash
-p
```

</details>

#### Required Tests

Test these URLs in your browser:

```text
http://YOUR_VM_EXTERNAL_IP/
http://YOUR_VM_EXTERNAL_IP/courses
http://YOUR_VM_EXTERNAL_IP/courses/c1
http://YOUR_VM_EXTERNAL_IP/courses/c10
```

Then test the same URLs in Postman.

#### Cleanup

When you finish, stop and remove the container.

Also check that no unnecessary containers are still running:

```bash
docker ps
```

When you finish the lab, go back to the GCP dashboard and stop your VM.

You can switch it on again when you need it.

#### Checklist

Before you finish, make sure:

- the Docker image builds successfully
- the container starts successfully
- the API works in the browser
- the API works in Postman
- unknown course IDs return `404`
- you can explain the difference between an image and a container
- your VM is stopped when you no longer need it

Part 3 is complete. Continue to [Homework 1](../homework/homework-1.md).
