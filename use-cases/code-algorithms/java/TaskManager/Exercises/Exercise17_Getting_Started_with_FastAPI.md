## Part 1: Understanding FastAPI Fundamentals

Source Language: Java

Target Framework: FastAPI (Python)

##  Part 1: Understanding FastAPI Fundamentals

# Prompt 1

What is FastAPI and how does it compare to Flask and Django?

Notes

FastAPI is a modern Python web framework used to build APIs quickly and efficiently.

Comparison:
# Feature	                    FastAPI	                   Flask	                          Django
Performance	                   Very Fast	               Moderate                        	   Moderate
API Development	               Excellent	               Good	                               Good
Validation	                   Built-in via Pydantic	   Manual	                           Forms/Serializers
Documentation	               Automatic	               Requires extensions	               Requires configuration
Learning Curve	               Moderate	                   Easy	                               Higher
Key Observation

FastAPI focuses on API development and high performance while automatically generating API documentation.

# Prompt 2

Explain the core concepts and terminology used in FastAPI.

Core Concepts
FastAPI Application
Routes
Path Parameters
Query Parameters
Request Body
Response Model
Pydantic Models
Dependency Injection
Middleware
Exception Handling


# Prompt 3

What are the key advantages of FastAPI over other Python web frameworks?

Advantages
High performance
Automatic API documentation
Type validation
Easy learning curve
Modern Python support
Built-in request validation


FastAPI Glossary

# Term	                             Meaning
Route	                             URL endpoint
Path Parameter	                     Variable inside URL path
Query Parameter	                     Optional value after ?
Pydantic	                         Validation library
Model	                             Structure of request/response data
Endpoint	                         API operation
Uvicorn	                             FastAPI web server
Dependency Injection	             Reusable component system

=========================================================================================================================================================================

## Part 2: Creating My First API
Prompt Used :Generate a basic Hello World example for FastAPI with comments explaining each part.

# Hello World Application:

from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}


# Running the Application
 Install Packages:
-pip install fastapi uvicorn

Start Server:
-uvicorn main:app --reload

Access URLs:
-http://127.0.0.1:8000

Automatic Documentation:
http://127.0.0.1:8000/docs



##  What I Learned
FastAPI automatically creates API documentation.
Routes are given using decorators.
Async functions are commonly used in FastAPI.

===========================================================================================================================================================================
## Part 3: Enhancing the API
Prompt Used: How do I add request body validation with Pydantic models in FastAPI?

# Example

from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float


# Benefits
Automatic validation
Type checking
Cleaner code
Better API documentation


# Prompt Used: Show me how to implement proper error handling in FastAPI.

# Example

from fastapi import HTTPException

@app.get("/items/{item_id}")
async def get_item(item_id: int):

    if item_id != 1:
        raise HTTPException(
            status_code=404,
            detail="Item not found"
        )

    return {"item_id": item_id}



# Prompt Used: How can I organize my FastAPI project into multiple files and modules?

Suggested Structure

my_fastapi_app/

├── app
│   ├── main.py
│   ├── routes
│   │   └── items.py
│   ├── models
│   │   └── item.py
│   └── utils
│       └── exceptions.py
│
└── requirements.txt


## What I Learned
Pydantic does validation automatically.
HTTPException is employed for error handling.
In bigger FastAPI projects, it is advisable to split them into modules.

===========================================================================================================================================================================

##  Part 4: To-Do List API Challenge

# Project Description
-A FastAPI application used to manage a to-do list.

# Features:

Create tasks
View tasks
Filter tasks
Mark tasks as completed
Delete tasks



# Pydantic Model

from pydantic import BaseModel
from datetime import date

class Todo(BaseModel):
    title: str
    description: str
    due_date: date
    completed: bool = False


# Create Task Endpoint

@app.post("/todos")
async def create_todo(todo: Todo):
    return todo

# List Tasks Endpoint
@app.get("/todos")
async def list_todos():
    return todos


# Complete Task Endpoint

@app.put("/todos/{todo_id}")
async def complete_todo(todo_id: int):
    todos[todo_id]["completed"] = True
    return todos[todo_id]

# Delete Task Endpoint
@app.delete("/todos/{todo_id}")
async def delete_todo(todo_id: int):
    del todos[todo_id]
    return {"message": "Deleted"}

# Filtering Example

@app.get("/todos/filter")
async def filter_todos(completed: bool):
    return [
        todo
        for todo in todos.values()
        if todo["completed"] == completed
    ]





## Reflection Questions
# What surprised you about FastAPI?
The automatic documentation generation was impressive in that it was a well-designed feature that is very easy to set up.


#  How did Java knowledge help?
Learning about classes, objects and API concepts helped to better comprehend Pydantic models and routes.

# Which AI prompts were most useful?

The prompts about:
FastAPI fundamentals
Pydantic validation
Project structure

were the most useful because they provided practical examples.

# What challenges did you encounter?

Understanding asynchronous functions and dependency injection required additional learning.

# What would you learn next?
Database integration
Authentication
Dependency Injection
SQLAlchemy
FastAPI testing


##  Key Learnings
FastAPI is optimized for modern API development and is also very fast.
Request validation is easier and code quality is enhanced with Pydantic.
FastAPI automatically generates interactive API documentation, making development and testing easier.