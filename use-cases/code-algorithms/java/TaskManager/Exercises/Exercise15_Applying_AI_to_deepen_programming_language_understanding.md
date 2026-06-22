##  Activity 1: Idiomatic Code Transformation

# Original Java Code
public int calculateTotal(List<Integer> numbers) {
    int total = 0;

    for (int i = 0; i < numbers.size(); i++) {
        total = total + numbers.get(i);
    }

    return total;
}

# Improved Idiomatic Java Code
public int calculateTotal(List<Integer> numbers) {
    return numbers.stream()
                  .mapToInt(Integer::intValue)
                  .sum();
}


# Comparison of Original vs Improved Code

Original Code	                                              Improved Code
Uses a traditional for loop with an index	                  Uses Java Streams for a more modern approach
Manually updates the total variable	                          Uses the built-in sum() method
More lines of code	                                          More concise and easier to read
More opportunity for indexing errors	                      Safer and follows Java best practices


# Why These Changes Follow Java Best Practices

The improved code uses the Java Stream API, which is a modern and idiomatic way of processing collections. It reduces boilerplate code, improves readability, and expresses the developer’s intention more clearly.

## Java Features Used
Stream API for processing collections.
Method references (Integer::intValue) for cleaner code.
Built-in aggregation methods such as sum().

## 3 Key Learnings from Activity 1
Java has in-built features like streams, that can eliminate repetitive loops.
When the code is clearly written with its intent, it can be much easier to understand, even if not as concise.
Knowing of some modern Java idioms makes one's code easier to read and maintain.




##  Activity 2: Code Quality Detective

# Original Java Code
public boolean checkUser(String username, String password) {
    if (username.equals("admin")) {
        if (password.equals("12345")) {
            return true;
        } else {
            return false;
        }
    } else {
        return false;
    }
}

# Code Quality Review
Identified Code Smells and Issues:

Unnecessary nested if statements.
Redundant else blocks after return.
Hard-coded credentials.
Poor readability due to excessive nesting.

# Improved Java Code

public boolean checkUser(String username, String password) {
    return "admin".equals(username) && "12345".equals(password);
}


# Why These Improvements Matter in Java

The improved version follows Java clean code principles by reducing nesting and making the condition easier to understand. Using "text".equals(variable) also prevents NullPointerException if the variable is null.

# Code Quality Rating

# Category	                          Rating	                              Reason
Readability	                          9/10	                                  The logic is clear and concise
Performance	                          9/10	                                  Fewer unnecessary checks and statements
Maintainability	                      8/10	                                  Easier to modify and understand


## The future Code Review Checklist will feature the following:
Don't have unnecessary nested conditions.
Write meaningful variable and method names.
Avoid duplicated code.
Where possible, break down complicated logic into simpler expressions.
Wherever possible, do not hardcode in values.
Use language-specific best practices.



## There are 3 key learnings from Activity 2:
There are simple things that can enhance readability.
Java has more safe coding patterns, like calling equals() on known constant values.
A code review checklist can help find common errors and enhance future code quality.





## Activity 3: Understanding a Java Language Feature

# Chosen Feature: Java Stream API

The Java Stream API allows developers to process collections of data using a functional programming style. Streams make operations such as filtering, mapping, and reducing collections more readable and expressive.

# Simple Example

# Traditional Approach

List<String> names = Arrays.asList("John", "Alice", "Bob");
List<String> result = new ArrayList<>();

for (String name : names) {
    if (name.startsWith("A")) {
        result.add(name);
    }
}

# Using Streams

List<String> result = names.stream()
                           .filter(name -> name.startsWith("A"))
                           .toList();

# Practical Use Cases

1. Filtering Data

Example: Finding all active users from a list of users.

2. Data Transformation

Example: Converting a list of objects into a list of names or IDs.

3. Calculations and Aggregations

Example: Calculating totals, averages, and maximum values from a collection.

# Small Practice Project Idea

Create a Student Management System that uses Java Streams to:

Filter students based on marks.
Calculate average marks.
Sort students by performance.
Extract student names into a separate list.

# Common Mistakes to Avoid
Using Streams for very simple operations where a loop is easier to understand.
Modifying collections while streaming over them.
Creating very long stream chains that reduce readability.
Forgetting that some Stream operations return a new collection rather than modifying the original data


# Sample Implementation Using Java Streams
import java.util.List;

public class StudentExample {

    public static void main(String[] args) {

        List<Integer> marks = List.of(50, 80, 90, 60, 75);

        List<Integer> passedStudents = marks.stream()
                                            .filter(mark -> mark >= 70)
                                            .toList();

        System.out.println(passedStudents);
    }
}

# AI Feedback on Implementation

The implementation correctly uses the Java Stream API to filter data. It is more concise than using a traditional loop and demonstrates a common real-world use of Streams.

Possible improvements include adding more operations such as sorting, mapping, or calculating averages.

## 3 Key Learnings from Activity 3
The Streams library is very expressive and convenient for processing collections in Java.
Lambda expressions and other features of functional programming help to make the code more concise and readable.
When adding features to the code, use advanced Java features only when they make the code more readable, and not just to make it more compact.
