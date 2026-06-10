
Initial Understanding of the Project

With a bit of research, and after reading the README file, I have a general idea that this is a command line-based task manager application. The app lets users create tasks, view, update, add tags, delete tasks and view statistics.

There are no web related files or frameworks, thus it is not a web application. Rather, users are able to use it by executing commands via the terminal.

Technologies Identified

From the project files, I have found the following technologies:

Java
Gradle is an automation tool that is used for building.
JUnit (for testing)
Command Line Interface (CLI)

The presence of build.gradle.kts, gradlew, and gradlew.bat indicates that Gradle is used to build and run the project.

Project Structure Analysis
Directory Structure
TaskManager
│
├── src
│   ├── main/java
│   │   ├── app
│   │   ├── cli
│   │   ├── model
│   │   ├── storage
│   │   └── util
│   │
│   └── test/java
│       ├── app
│       └── util
│
├── build.gradle.kts
├── settings.gradle.kts
├── README.md
└── gradle
The main folders and their function.

Folder	                Purpose

app	               Contains the main application logic
cli	               Processes command-line arguments and user input/output
model	               Defines the data models that are used by the application.
storage	               Responsible for storage and management of task data
util	               Helper classes and utility functions.
test	               Automated tests for application components


Main Components Identified

1. TaskManager.java (app)
-This is most likely the primary application controller. Given the name, it's probably used to handle coordination of tasks and to interact with other classes.

2. TaskManagerCli.java (cli)
-This class seems to be in charge of processing user commands in the command line.

3. Task.java (model)
Represents an individual task.

4. TaskPriority.java (model)
-The priority levels to work with for tasks are determined in the following way: likely.

5. TaskStatus.java (model)
-Probably describes the various aspects of a task, pending or completed.

6. TaskStorage.java (storage)
-Looks attentive to assignments that need to be made and completed.

7. Utility Classes
The package util includes the following:

TaskMergeService.java
TaskPriorityManager.java
TaskTextParser.java

These classes would seem to be supporting classes for the management and processing of tasks.


Application Entry Point

My initial guess was the entry point is in the App package, in some place.

Based on the structure, I think the flow of execution was probably:

User Command
      ↓
TaskManagerCli
      ↓
TaskManager
      ↓
TaskStorage / Model Classes

The CLI class takes user instructions, sends them to the TaskManager class, and the TaskManager carries out the needed operations on the tasks.



Architectural Pattern Observed

The project seems to be composed of layers:

CLI Layer
    ↓
Application Layer
    ↓
Storage Layer
    ↓
Model Layer

This separation makes it easier to maintain code as each package has its own responsibility.



Misconceptions and Corrections

Misconception 1
-The initial idea was to come up with a web application since task management systems are generally web based.

Correction:
-From reading the README and the structure of the folder, this is clearly a command line tool.


Misconception 2
-I assumed that there would be a database since tasks should be saved.

Correction:
-The project structure does not have any database configuration files and only has a TaskStorage class. This is indication that task storage is done in memory instead of using a database.

Misconception 3
-I assumed that everything to do with the task was in a single big class.

Correction:
-The project is divided into several packages and classes, this makes the project code more organized.

Questions that I would ask the team:
So, what is the exact entry point of the application?
What is the process for how tasks are stored during runtime of the application?
What does TaskMergeService utility do?
Why has a command-line interface been used instead of a graphical interface?
Do you plan to have a persistent store like database in the future?
Small Exploration Exercise


I would like to make improvements to my understanding of the project by:

Open TaskManagerCli.java.
Trace how the create command is processed.
Look at the flow and trace it to TaskManager.java.
See how a new Task object is created.
To check storage of task use TaskStorage.java.

This would enable me to gain insight into the interactions between the various components of the system.