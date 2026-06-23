## Part 1: Analyzing Complex Code

# Repository Pattern

What is it?

The Repository Pattern creates a layer between the application and the database.

Instead of API endpoints directly querying the database, they communicate with a repository object.

# 
In this code
class Repository(Generic[T]):
-This class provides common database operations:

get_by_id()
list()
-The UserRepository extends it:

class UserRepository(Repository[User]):

and adds user-specific operations:

get_by_username()

## Benefits

Separates database logic from business logic
Makes code easier to test
Reduces duplicate database code
Easier to switch databases later

# Without Repository Pattern

@app.get("/users/{id}")
async def get_user(id: int):
    result = await db.execute(...)

Database code would be repeated everywhere.

# With Repository Pattern
user = await user_repo.get_by_id(db, id)

Much cleaner.

## Generic[T]

# What does it mean?
T = TypeVar('T', bound=Base)

T acts as a placeholder for any database model.

Instead of creating:

UserRepository
ProductRepository
OrderRepository

with duplicated code, one generic repository can handle all models.

# Example
user_repo = Repository(User)
product_repo = Repository(Product)

Both use the same methods:

get_by_id()
list()


# Benefits
Reusable code
Less duplication
Easier maintenance
Better type checking


## Dependency Injection

FastAPI automatically provides required objects.

# Database Dependency

db: AsyncSession = Depends(get_db)

FastAPI executes:

get_db()

and injects the session.

# Authentication Dependency

current_user: User = Depends(get_current_user)

FastAPI:

Reads JWT token
Validates token
Finds user
Returns authenticated user

before endpoint execution.

# Dependency Chain
Request
   ↓
OAuth2PasswordBearer
   ↓
get_current_user()
   ↓
get_db()
   ↓
Database

FastAPI automatically resolves everything.

# Benefits
Cleaner code
Reusable authentication
Reusable database access
Easier testing

## Role-Based Access Control

# Purpose

Restrict endpoints based on user roles.

# Decorator
@requires_role("admin")

# Process

When endpoint is called:

if not current_user.is_superuser:

then:

403 Forbidden

is returned.


# Benefits
Security logic centralized
Reusable across endpoints
Cleaner route handlers


=========================================================================================================================================================================

## Part 2: Execution Flow for /admin/users/

# Sequence Diagram


Client Request
      |
      v
TimingMiddleware
      |
      v
requires_role("admin")
      |
      v
Depends(get_current_user)
      |
      v
OAuth2PasswordBearer
      |
      v
Extract JWT Token
      |
      v
JWT Decode
      |
      v
Depends(get_db)
      |
      v
Create Database Session
      |
      v
UserRepository.get_by_username()
      |
      v
User Found?
      |
      +---- No --> 401 Unauthorized
      |
      v
Check is_superuser
      |
      +---- No --> 403 Forbidden
      |
      v
Execute list_users()
      |
      v
UserRepository.list()
      |
      v
Query Database
      |
      v
Return Users
      |
      v
TimingMiddleware adds X-Process-Time
      |
      v
Response Sent

## Authentication Flow

# Step 1

Request contains:

Authorization: Bearer eyJ...

# Step 2
oauth2_scheme

extracts token.

# Step 3
jwt.decode()

verifies token.

# Step 4
get_current_user()

finds user in database.

# Step 5
requires_role("admin")

checks permissions.

# Step 6

Endpoint executes.

========================================================================================================================================================================

## Part 3: Translation Guide

# asynccontextmanager + lifespan

# Technical View

@asynccontextmanager
async def lifespan(app):

runs startup and shutdown logic.

# Simple Explanation

Think of it as:

Program starts
↓
Setup resources
↓
Application runs
↓
Clean up resources
↓
Program stops


# Example

Startup:

Connect to database

Shutdown:

Close database connection


##  TimingMiddleware

# Purpose

Measure request duration.

# Simple Example

User requests:

GET /users

Timer starts:

start_time

Endpoint executes.

Timer stops:

process_time

Header added:

X-Process-Time: 23.5

Useful for monitoring performance.



## JWT Authentication (Simple Version)

# Login

User submits:

username
password


# Server

Verifies credentials.

Creates token:

jwt.encode(...)

# Client

Stores token.

# Future Requests

Client sends:

Authorization: Bearer TOKEN

# Server

Checks token validity.

If valid:

Access Granted

If invalid:

401 Unauthorized

Think of JWT as a digital access card.

===========================================================================================================================================================================

# Part 4: Logging System Implementation

# Step 1: Create Log Model

class UserLog(Base):
    __tablename__ = "user_logs"

    id: int
    username: str
    action: str
    timestamp: datetime

# Step 2: Create Repository

class LogRepository(Repository[UserLog]):

    async def create_log(
        self,
        db,
        username,
        action
    ):
        log = UserLog(
            username=username,
            action=action,
            timestamp=datetime.utcnow()
        )

        db.add(log)
        await db.commit()

        return log


# Step 3: Create Service
class LogService:

    def __init__(self, repository):
        self.repository = repository

    async def record_action(
        self,
        db,
        username,
        action
    ):
        return await self.repository.create_log(
            db,
            username,
            action
        )


# Step 4: Log Login Events

Inside:

login()

add:

log_repo = LogRepository(UserLog)
log_service = LogService(log_repo)

await log_service.record_action(
    db,
    user.username,
    "LOGIN"
)

# Step 5: Log Admin Actions

Inside:

list_users()

add:

log_repo = LogRepository(UserLog)
log_service = LogService(log_repo)

await log_service.record_action(
    db,
    current_user.username,
    "VIEW_ALL_USERS"
)

## Reflection Questions
1. How does implementing this feature help you understand the architecture?
It shows how the application is divided into layers:

Routes → handle requests
Services → business logic
Repositories → database access
Dependencies → shared resources

2. Which design patterns were most useful?
Repository Pattern
Dependency Injection
Service Layer Pattern
Middleware Pattern

3. How would you explain Repository Pattern and Dependency Injection to a colleague?

Repository Pattern
A repository is a dedicated class responsible for database operations so that the rest of the application doesn't need to know how data is stored.

Dependency Injection

Dependency Injection lets FastAPI automatically provide objects like database sessions and authenticated users instead of manually creating them.

4. How did tracing execution flow help?

Tracing the flow revealed:

Middleware:
Authentication
Authorization
Route
Repository
Database

## Three Key Learnings:
Dependency Injection allows FastAPI to automatically manage resources such as database sessions and authenticated users.
Repository Pattern separates database access from business logic, making applications easier to maintain and test.
Middleware and decorators provide reusable ways to add cross-cutting functionality such as timing, authentication, authorization, and logging without changing endpoint code.