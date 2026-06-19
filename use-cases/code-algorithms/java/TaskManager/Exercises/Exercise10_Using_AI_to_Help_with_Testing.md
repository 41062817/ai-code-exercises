## Chosen Language: Java
Project: Task Manager – TaskPriorityManager.java

## Part 1: Understanding What to Test
# Exercise 1.1: Behavior Analysis

AI Conversation Summary:

Question 1: What does calculateTaskScore() do?

The function calculates the importance score of a task based on several factors:

Task priority level
Due date urgency
Task status
Special tags
Recent updates

Question 2: What edge cases should be tested?

Possible edge cases include:

Tasks with no due date
Overdue tasks
Tasks due today
Completed tasks receiving a negative score
Tasks containing special tags
Recently updated tasks
Tasks with no tags
Tasks with low and urgent priorities

Question 3: Which test should be written first and why?

The first test should verify the base priority score because it is the foundation of all other calculations. If priority scoring is incorrect, all final task scores will be incorrect.



# Test Cases to Write
-Test Case 1: Ensure base priority score is achieved

Input: HIGH – priority task
Expected Result: Base score should be 30 points.

-Test Case 2: Check overdue task bonus

Input: Task with an earlier due date than today
Extra 30 points: The task is carried out as expected.

-Test Case 3: Check if penalty is applied for completed task.

Input: DONE task
Expected Result: 50 points will be subtracted from the score.

-Test Case 4: Identify any copyright issues.Test Case 6: Ensure that the score includes critical tags.

A task with a critical” tag.A task with the tag critical”
Expected Result: The score increases by 8 points.

Test Case 5: Check bonus upon recent update

Less than one day old task: Input
Expected Result: The score increases by +5 points.


##  Exercise 1.2: Test Planning Document

# Testing Priority
# Priority	     Test	                                       Type	                          Expected Outcome
High	         Priority score calculation	                    Unit Test	                  Correct base score is returned
High	         Due date calculations	                        Unit Test	                  Correct due date bonuses are applied
High	         Status penalties	                            Unit Test	                  DONE and REVIEW reduce score correctly
Medium	         Tag boosts	                                    Unit Test	                  Special tags add 8 points
Medium	         Recent update boost	                        Unit Test	                  Recently updated tasks add 5 points
High	         Task sorting	                                Integration Test	          Tasks are sorted from highest to lowest score
High	         Top priority selection	                        Integration Test	          Only highest scoring tasks are returned


# Test Dependencies

The tests depend on:

Task class
TaskPriority enum
TaskStatus enum
TaskPriorityManager methods



============================================================================================================================================================================

## Part 2: Improving a Single Test
# Exercise 2.1: Basic Test

Simple Test


public void testHighPriorityScore() {
    Task task = new Task("Complete assignment");
    task.setPriority(TaskPriority.HIGH);

    int score = TaskPriorityManager.calculateTaskScore(task);

    assertEquals(30, score);
}


Improved Test

The simple test only checks priority. The improved version makes the purpose clearer and controls other variables.


public void shouldCalculateCorrectScoreForHighPriorityTask() {
    Task task = new Task("Complete assignment");
    task.setPriority(TaskPriority.HIGH);
    task.setStatus(TaskStatus.TODO);
    task.setTags(new ArrayList<>());
    task.setDueDate(null);

    task.setUpdatedAt(LocalDateTime.now().minusDays(2));

    int score = TaskPriorityManager.calculateTaskScore(task);

    assertEquals(30, score);
}


# Exercise 2.2: Due Date Calculation Test
Good Testing Principles

A good test should:
-Test each behaviour separately.
-Control unrelated variables
-Make sure you know what results you want
-Verify boundary conditions


# Due Today Test
 
public void shouldIncreaseScoreForTaskDueToday() {

    Task task = new Task("Submit report");

    task.setPriority(TaskPriority.MEDIUM);
    task.setDueDate(LocalDateTime.now());
    task.setUpdatedAt(LocalDateTime.now().minusDays(2));

    int score = TaskPriorityManager.calculateTaskScore(task);

    // 20 base + 20 due today
    assertEquals(40, score);
}


==========================================================================================================================================================================

##  Part 3: Test Driven Development (TDD)

# Exercise 3.1: New Feature - Current User Boost (+12)

-Step 1: Write Failing Test

public void shouldAddBonusForAssignedCurrentUser() {
    Task task = new Task("Fix bug");

    task.setAssignedUser("currentUser");

    int score = TaskPriorityManager.calculateTaskScore(task);

    assertTrue(score >= 12);
}

-Step 2: Implement Minimal Code Fix

Add to calculateTaskScore():

if(task.getAssignedUser().equals("currentUser")) {
    score += 12;
}

-Step 3: Refactor

Move the user comparison to a separate method or constant to improve readability.


# Exercise 3.2: TDD Bug Fix - Days Since Update

-Failing Test

public void shouldCalculateDaysSinceUpdateCorrectly() {

    Task task = new Task("Update documentation");

    task.setUpdatedAt(LocalDateTime.now().minusDays(3));

    int score = TaskPriorityManager.calculateTaskScore(task);

    assertFalse(score >= 5);
}



Fix

The existing code already uses:

ChronoUnit.DAYS.between(
    task.getUpdatedAt(),
    LocalDateTime.now()
);

*This is the correct implementation.

*The test verifies that tasks older than one day do not receive the recent update bonus.

=========================================================================================================================================================================

## Part 4: Integration Testing

Testing Full Workflow

This test verifies that all three methods work together.


public void shouldReturnHighestPriorityTasks() {

    Task task1 = new Task("Low task");
    task1.setPriority(TaskPriority.LOW);

    Task task2 = new Task("Urgent task");
    task2.setPriority(TaskPriority.URGENT);

    List<Task> tasks = List.of(task1, task2);

    List<Task> result =
        TaskPriorityManager.getTopPriorityTasks(tasks, 1);

    assertEquals(1, result.size());

    assertEquals(
        TaskPriority.URGENT,
        result.get(0).getPriority()
    );
}



## Reflection
What I learned about testing
-The idea of this exercise was to demonstrate that it is important to understand the behaviour of a function before writing tests for it. AI proved valuable to help identify key scenarios and edge cases.

-I found that the unit test should be used to check a single behaviour, like calculating priorities or handling due dates, and integration tests should be used to check that several functions work together properly.

-The TDD process helped me to realize the value of writing a failing test first, instead of modifying the code. It also demonstrated that code must be modified to make the test pass, but no further than that.

-Instead of just generating all tests automatically, in the future I would use AI to direct my testing strategy, identify edge cases that I am missing, and the quality of my assertions would be enhanced.

