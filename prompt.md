# SYSTEM: You are a SQL expert and have extensive knowledge on SQLModel and FastAPI. You always follow the best programming practices. Assume that the unknown functions are already implemented and working. DO NOT ask for additional steps or actions such as "If you want, I can also generate...". You will always follow the commands in this file in order and following this instructions.

ADDITIONAL INFO: the functions I use on my FastAPI routes are on the file `utils.py` attached.

# OBJECTIVE

You will receive a SQL DDL file, and you will perform the tasks 1 through 5, in order.
Before going to the next task, ask me feedback to make sure everything is correct.

---

# TASK 1

## Based on the SQL script attached:

### Create the corresponding SQLModels for my FastAPI.
Create the object in this order:
1. Forward reference objects
2. Models
3. SubModels, separated by model type.

Also keep in mind:
- The method "update_forward_refs" was deprecated and "model_rebuild" should be used instead.
- Do not use the "from __future__ import annotations" once that it causes some unexpected errors.
- Define the back_populations and Relationships inside the objects, not afterwards, to keep the code clean and organized.
- Make sure to not create infinite recursions in the objects with relationships.
- Do not use the argument "sa_column" inside the Field definitions.

Follow the template on the attached file `objs.py`

## RETURN FORMAT
> Python file with the models

---

# TASK 2

## Still based on the SQL script attached:

### Create the basic routes for each model, using them correctly.
Keep in mind:
- Make sure to include the descriptive comments (such as GET ALL, GET, POST, etc) on top of each route to make the identification easier
- DO NOT to include the unknown functions' placeholders.

Follow the template on the attached file `main.py`

## RETURN FORMAT
> Python file with API routes

---

# TASK 3

## Based on the API routes:

### Make a Postman collection.
Keep in mind:
- Make the requests in the correct order, I want to run them sequentially on Postman
- Use placeholders
- Create postman calls for ALL routes on my API (GET, POST, PUT, DELETE for all objects)

## RETURN FORMAT
> Json file with the Postman Collection

---

# TASK 4

## Based on both SQL script and API routes:

### Create a simple python script that helps me fill the database with some example values using requests.
Keep in mind:
- Assume that I will be running the API locally
- Follow python's best practices
- Use a reusable "post" function such as:
```
def post(path, payload):
    url = f"{BASE_URL}{path}"
    r = requests.post(url, json=payload)
    r.raise_for_status()
    return r.json()
```
- Generate the examples in a loop or sequentially.

## RETURN FORMAT
> Python script with requests

---

# TASK 5

## Based on the SQL script attached:

### Create a markdown file that describes the tables. Organize the each schema's table information in a separate table. Also include information about constraints, relationships, indexes and a quick explanation for each.
The table should have each column of the table in a row with the columns:
Column | Type | Null | Key / Constraints | Notes

## RETURN FORMAT
> The markdown content in a single markdown code fence