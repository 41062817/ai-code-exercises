Original Code Chosen

Algorithm: Task Priority Sorting Algorithm

Language: Java

public static int calculateTaskScore(Task task) {
    Map<TaskPriority, Integer> priorityWeights = Map.of(
        TaskPriority.LOW, 1,
        TaskPriority.MEDIUM, 2,
        TaskPriority.HIGH, 3,
        TaskPriority.URGENT, 4
    );

    int score = priorityWeights.getOrDefault(task.getPriority(), 0) * 10;

    if (task.getDueDate() != null) {
        long daysUntilDue = ChronoUnit.DAYS.between(LocalDateTime.now(), task.getDueDate());

        if (daysUntilDue < 0) {
            score += 30;
        } else if (daysUntilDue == 0) {
            score += 20;
        } else if (daysUntilDue <= 2) {
            score += 15;
        } else if (daysUntilDue <= 7) {
            score += 10;
        }
    }

    if (task.getStatus() == TaskStatus.DONE) {
        score -= 50;
    } else if (task.getStatus() == TaskStatus.REVIEW) {
        score -= 15;
    }

    if (task.getTags().stream().anyMatch(tag ->
            List.of("blocker", "critical", "urgent").contains(tag))) {
        score += 8;
    }

    long daysSinceUpdate = ChronoUnit.DAYS.between(task.getUpdatedAt(), LocalDateTime.now());
    if (daysSinceUpdate < 1) {
        score += 5;
    }

    return score;
}




Documentation Generated Using Prompt 1:
/**
 * Calculates an importance score for a task based on different factors
 * such as priority level, due date, completion status, tags, and how
 * recently the task was updated.
 *
 * A higher score means the task should be treated as more important.
 *
 * @param task The Task object containing information such as priority,
 *             due date, status, tags, and update time.
 *
 * @return An integer score representing the overall importance of the task.
 *         Higher values indicate higher importance.
 *
 * @throws NullPointerException May occur if the task object or required
 *         task fields are null.
 *
 * Example:
 * Task task = new Task("Complete assignment");
 * int score = calculateTaskScore(task);
 *
 * Important notes:
 * - Overdue tasks receive additional points.
 * - Tasks due soon receive a smaller score increase.
 * - Completed tasks receive a large score reduction.
 * - Tasks with tags like "urgent" or "critical" receive extra points.
 * - Recently updated tasks receive a small score boost.
 */



 Insights and Improvements Identified Using Prompt 2:

 High-Level Purpose
 This function is designed to assign a score to a task, to indicate its importance. It doesn't just look at the task priority, but it takes into account several things to make a smart decision on which tasks should show up first.

 
 Step-by-Step Logic:
 1. The algorithm starts by assigning a base score according to the task priority:
LOW = 10 points
MEDIUM = 20 points
HIGH = 30 points
URGENT = 40 points

2. It checks the due date of the task:
Overdue tasks receive +30 points.
Tasks due today receive +20 points.
Tasks due within 2 days receive +15 points.
Tasks due within a week receive +10 points.

3. It checks the task statu:
Completed DONE tasks are deducted 50 points.
Tasks that are close to completion take away 15 points from the REVIEW category.

4. It validates the task tags:
Each task with a 'blocker', 'critical' or 'urgent' tag is given an extra 8 points.

5. Checks last update date:

Tasks are updated recently and given 5 additional points.


6. The final score is given, and can be ranked from highest to lowest priority.


Suggested Inline Comments:
// Give a starting score based on task priority

// Increase score depending on how close the due date is

// Reduce importance for completed or review tasks

// Increase score for special high importance tags

// Give a small boost to recently updated tasks

// Return the final calculated importance score


Possible Improvements:
The values of the scores (e.g., +30 for due tasks) might be represented as named constants to make them easier to modify later.
Further validation may be included to avoid possible errors when it comes to a task that lacks information.
The algorithm can be configured such that various users could select their own priority rules.



 Reflection:

Which parts of the documentation were most challenging for the AI?

-The most difficult aspect was finding any possible error or assumption as there is no specific check in the algorithm to determine whether correct and all of the task information is provided. You had to have some idea of how the Task object should behave.

What other information did you need to give in your prompts?

-I had to give the entire function code and let them know that it was part of a Java Task Manager application. This aided the AI in comprehending the function of the algorithm and in creating more reliable documentation.

What would you do with this method in your own work?

-I, as a novice programmer, would utilize AI to write basic documentation and for complex methods. But still check the documentation against the code to ensure that it accurately reflects the action of the program.