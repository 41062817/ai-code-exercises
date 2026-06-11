Part 1: Understanding Project Structure



Initial Understanding

First glance at project was the folder structure and README file. My first thought was that it was a Task Manager program written in Java that lets the users make, update, list and delete tasks from the command line.

I found the following technologies:

Java
Gradle
Command Line Interface (CLI)

I assumed there would be classes that would be responsible for task management and data storage, but I wasn't sure how tasks are stored.

Final Understanding

I went through the app's code, and with a little help from the AI prompts, got a better idea of the app's architecture.

The project is broken into the following packages:

Package	               Responsibility

The application's      business logic for task management.
cli	                   Processes user commands and is the entry point for programs
Model                  domain entities for e.g. Task, TaskStatus, TaskPriority
storage	               Reads and writes the data of tasks to/from JSON files


Technologies and Libraries
The project uses:

Java
Gradle
Apache Commons: CLI (command-line argument processor)
Gson (JSON serialization/deserialization)
Application Entry Point

The application will now start in:

TaskManagerCli.main(String[] args)

This approach takes user commands and passes them to appropriate handler methods.



Architecture

The application layer is organized in layers:

User
 ↓
TaskManagerCli
 ↓
TaskManager
 ↓
TaskStorage
 ↓
tasks.json

The CLI handles the parsing of user commands, the TaskManager does the business logic and the TaskStorage handles the persistence of the Tasks, saving them to a JSON file.




Misconceptions

Initially I thought:

The project only had tasks stored in memory.
The application may be a single main class with a lot of functionality.

After doing some exploration of the code, I found that:

Tasks are stored in a tasks.json file with the help of Gson.
Responsibilities are separated into different packages and classes.



Key Components

TaskManager
Responsible for:

Creating tasks
Updating status
Updating priority
Updating due dates
Managing tags
Producing statistics


TaskManagerCli
Responsible for:

User's input for processing
Displaying output
Acting as the application's entry point


Task
Represents a task and has:

ID
Title
Description
Priority
Status
Due date
Tags
Creation date
Completion date


TaskStorage
Responsible for:

Saving tasks to JSON.
These are loaded from a JSON file.They are loaded from a JSON file.
Retrieving tasks
Updating tasks
Deleting tasks



Part 2: Finding Feature Implementation

Scenario:
The team lead asked for a new feature Task Export to CSV.

Initial Search
I looked up information about:

file operations
save
load
storage
JSON handling

The file found which seemed to be the relevant one was:
TaskStorage.java
It is already used for reading and writing task data, so it is suitable for this.


Hypothesis:
I thought it was important to export the data in the task close to the storage layer because in order to export the data you need to get access to the stored task data.

Findings
The code base already has some functionality for:

The JSON is used to load tasks.Tasks are loaded from JSON.
Saving tasks to a JSON file.
Retrieving all tasks
This implies that the export feature may be able to use existing storage features.


Components Affected
These components should probably be changed:

TaskManagerCli
TaskManager
TaskStorage


Implementation Plan
Add a new command, export.
The tasks will be handled and added to TaskManagerCli.
Make up a way to start the exporting from TaskManager.
Fetch all of the tasks in TaskStorage.
Write the data of tasks to CSV.
Save CSV data in a file, like tasks.csv.
Show a success message when export successfully done.


Part 3: Understanding Domain Model


Core Entities

The main domain entities are:

Task
TaskStatus
TaskPriority
Entity Relationship Diagram
Task
 ├── TaskStatus
 └── TaskPriority

Entity Descriptions
Task
The Task entity is the entity that contains one work item that is handled by the application.

A task contains:
Unique ID
Title
Description
Status
Priority
Due date
Completion date
Tags


TaskStatus :Represents the state in which a task is currently.

Some code examples are as follows:
TODO
IN_PROGRESS
REVIEW
DONE


TaskPriority: Represents how important a task is.

Examples include:
LOW
MEDIUM
HIGH
URGENT


Business Logic Identified
It is important to note that the Task class has some of the key business rules:

Task is automatically set to TODO when created.
MarkAsDone() is used to mark a task as completed.
If the due date of a task is passed and it is not marked DONE, then it is considered an overdue task.
Duplicate tags are not allowed.



Domain Glossary
Term	                        Meaning
 Task                            A work item that is managed by the system.
 Status                          The state of a task (open, active, closed, etc.).
Priority	                     Importance of a task
Due Date	                     Deadline for completing a task
Tag	                             Label for a task that you can attach to it.
Overdue Task:                    A task whose due date has come and gone.



Questions and Answers
Question: What determines whether a task is overdue?
Answer: The deadline for the task.
A task is late if it has a due date that is set prior to the current date and is not marked as DONE.

Question: What happens when a task is marked as done?
Answer: When a task is done, it is removed from the list.
Answer: It transitions to DONE and the ‘done' date is marked.

Question: Why does the system use TaskPriority?
Answer: TaskPriority is used in the system because of the following reasons:
Answer: To enable tasks to be sorted by importance.


Part 4: Practical Application

New Business Rule
Tasks not completed for more than 7 days should be automatically considered abandoned, unless they are indicated as high priority.

Files I Would Modify

My understanding of the code base is that I would look at:

Task.java
TaskManager.java
TaskStorage.java
TaskStatus.java


Planned Changes

Implement a new status for TaskStatus: ABANDONED
Write logic to determine if task is > 7 days overdue.
Verify if the task priority is "high" or "urgent.
If the task is overdue and not high priority, its status is automatically changed to ABANDONED.
Apply with TaskStorage to save the updated task.



Questions to ask the Team
Should left tasks be modifiable?
Should a warning be given to users before a task is abandoned?
Are URGENT tasks to be out of the rule as well?
At what time is the overdue check due?



Final discussion and reflection.
The most valuable insights gained were:

Prompt 1: Understanding Project Structure

This prompt gave me an understanding of the general architecture without needing to read the implementation details as it first presented the ideas and then gave examples of them. It was good to see how the various packages link together.

Prompt 2: Finding Feature Implementation

The question was very helpful to me in identifying how to find functionality by following related features and identifying reusable components.

Prompt 3: How do you define a domain model.

This prompt gave me the opportunity to think about the business implications of the code and not just the technical.

How the AI Prompts Helped

The AI provided structured approach to understanding codebase. This allowed me to delve into different topics, such as project structure, location of features, and domain concepts, rather than reading through files randomly.



Remaining Uncertainties

I don't know how to do:

How the application would scale with a large number of tasks.
Did there exist other status values besides those I explored in my files?
How the future features should be structured as the application develops.



Future Strategy

In the future, I will approach an unknown codebase and:

Read the README before using this.
Locate the application start point.
Discuss the structure of the project.
Find business rules and core entities.
Follow a user input to data storage feature.
Use AI to check and enhance my knowledge.