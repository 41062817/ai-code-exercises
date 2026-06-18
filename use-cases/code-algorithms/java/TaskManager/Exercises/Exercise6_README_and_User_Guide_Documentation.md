## Project Information Chosen

Project Name: Task Manager

Description:
Task Manager is a Java based command-line application which enables users to create, organize, update and manage their tasks. Users can write down tasks, mark them with the description, priority, due date and tags. It also enables the user to monitor progress by updating the statuses of tasks and viewing the statistics of tasks.

Key Features:

Make new tasks titled, described, prioritized, with a due date and tags
View a list of all tasks or filter tasks by status and priority
Update task status, task priority, and due dates
Add and remove tags to tasks.
Display more information about a task
Mark tasks as completed if no longer required
Display task statistics

## Technologies Used:
-Java
-Gradle

## Installation Requirements:

-Java Development Kit (JDK)
-Gradle (or use the provided Gradle wrapper)
## Comprehensive README.md (Prompt 1)

Task Manager
Description:
Task Manager is a simple command-line application built in Java for managing personal or project tasks. It helps users keep track of tasks by allowing them to create, update, organize, and delete tasks.

## Features
Create tasks with a title, description, priority, due date, and tags
List all tasks or filter them based on status or priority
Change the status of a task
Update task priority and due dates
Add and remove tags from tasks
View detailed information about a task
Delete completed or unnecessary tasks
View task statistics



## Installation
Prerequisites:

Before running the application, make sure you have:

-Java Development Kit (JDK) installed
-Gradle installed or the Gradle wrapper included in the project


## Build the Project

Run the following command:

./gradlew build

## Running the Application

Use the following command:

./gradlew run --args="<command>"



##  Available Commands

Command	                                                                           Description

create <title> [description] [priority] [due_date] [tags]	                       Creates a new task
list [-s <status>] [-p <priority>] [-o]	                                           Displays all tasks or filters them
status <task_id> <new_status>	                                                   Changes the status of a task
priority <task_id> <new_priority>	                                               Updates the priority of a task
due <task_id> <new_due_date>	                                                   Updates a task due date
tag <task_id> <tag>	                                                               Adds a tag to a task
untag <task_id> <tag>	                                                           Removes a tag from a task
show <task_id>	                                                                   Shows detailed information for a task
delete <task_id>	                                                               Removes a task
stats	                                                                           Displays task statistics


## Usage Examples

Create a task:

./gradlew run --args="create 'Fix Code' 'I need to get this running' 1 2025-06-01 bugs"

List all tasks:

./gradlew run --args="list"

## Configuration

No additional configuration is required before using the application. The user only needs a working Java environment and the project dependencies provided by Gradle.


## Troubleshooting
-Application does not run
-Make sure Java is installed correctly.
-Verify that Gradle has been installed or use the included Gradle wrapper.
## Build errors
-Ensure all project files and dependencies are available.
-Run the build command again to download missing dependencies.
## Invalid commands
-Check the command format and make sure all required arguments are included.

## Contributing

Any donations to enhance the Task Manager application are appreciated. When changes are being made, developers should ensure changes are tested prior to submission of update to ensure that new features are organized in the same way as the rest of the project.

## License

The intent of this project is to be used for educational purposes. Review individual project repository for any licensing restrictions.



## Step-by-Step Guide (Prompt 2)

How to Create and Manage Tasks in Task Manager
Prerequisites:

Before starting, ensure that:

Java is installed on your computer.
The Task Manager project has been built successfully using Gradle.


# Step 1: Build the Application

Open a terminal in the project directory and run:

./gradlew build

# Step 2: Create a New Task

Use the create command:

./gradlew run --args="create 'Complete Assignment' 'Finish the software development exercise' 2 2026-06-30 school" 
-This creates a task with a title, description, priority, due date, and a tag.

# Step 3: View Existing Tasks

To display all tasks, run:

./gradlew run --args="list"

# Step 4: Update a Task Status

To mark a task with a different status:

./gradlew run --args="status 1 DONE"

# Step 5: Change Task Priority

Use the priority command:

./gradlew run --args="priority 1 HIGH"

# Step 6: Add or Remove Tags

Add a tag:

./gradlew run --args="tag 1 urgent"

Remove a tag:

./gradlew run --args="untag 1 urgent"

# Step 7: View Task Details

To see complete information about a specific task:

./gradlew run --args="show 1"

## Troubleshooting
The command does not work:
-Be sure to run the command from the project folder.
-Verify for spelling of command and arguments.

Task ID not found:
-Prior to editing or deleting a task, ensure that the task is present by using the list command.




##  FAQ Document (Prompt 3)
Task Manager Frequently Asked Questions (FAQ)
Getting Started

What is Task Manager?
-Task Manager is a Java command-line application used to create and organize tasks using commands in the terminal.

# How do I build the application?

Run:

./gradlew build

How do I start using Task Manager?

After building the application, run commands using:

./gradlew run --args="<command>"

# Task Management
How do I create a task?

Use the create command:

./gradlew run --args="create 'My Task' 'Task description' 1 2026-07-01 work"

# How can I view all my tasks?

Use:

./gradlew run --args="list"
How do I change the status of a task?

Use:

./gradlew run --args="status <task_id> <new_status>"

Example:

./gradlew run --args="status 1 DONE"

# How do I delete a task?

Use:

./gradlew run --args="delete <task_id>"

Example:

./gradlew run --args="delete 1"

## Common Problems
The application fails to run:

-Check that Java is installed and that the project was built successfully.

A command returns an error:

-Make sure the command format is correct and that all required values such as task IDs or statuses are provided.

Can I recover a deleted task?

-No. Once a task is deleted, it must be created again.


