##  Exercise 1: Code Readability Improvement (Java)

Readability Issues Identified
1. Unclear Class and Variable Names

The code uses short and unclear names that make understanding the purpose difficult.

# Original Name	    Improved Name	                                 Reason
UserMgr	            UserManager	                                     Clearly describes the class purpose
U	                User	                                         Makes the model understandable
u_list	            users	                                         Follows Java naming conventions
db	                databaseConnection	                             More descriptive
a()	                addUser()	                                     Explains the action performed
f()	                findUserByUsername()	                         Clearly describes the search operation
un	                username	                                     Easier to understand
pw	                password	                                     More readable
em	                email	                                         Uses complete naming


2. Function Structure Improvements

The addUser() function performs multiple tasks:

-Validates user input
-Checks if the username already exists
-Creates a user object
-Stores the user in memory
-Inserts the user into the database

It can be broken into smaller helper methods:

isValidUserData()
isUsernameTaken()
saveUserToDatabase()

This makes the code easier to read, test, and maintain.

3. Additional Problems Found
SQL statements are created using string concatenation, which can lead to SQL injection attacks.
The database operation should use parameterized queries.
The database insertion result should be checked before adding the user to the list.
Getter names such as getUn() and getPw() should be renamed to getUsername() and getPassword().

Example of Improved Structure:

public boolean addUser(String username, String password, String email) {
    if (!isValidUserData(username, password, email)) {
        return false;
    }

    if (isUsernameTaken(username)) {
        return false;
    }

    User user = new User(username, password, email);

    if (saveUserToDatabase(user)) {
        users.add(user);
        return true;
    }

    return false;
}

# What I Learned:
Use meaningful names for better code readability.
There should be one clear responsibility in a method.
Security issues may be also identified during the refactoring process.
Smaller methods are easier to test and keep up.

=============================================================================================================================================================================

## Exercise 2: Function Refactoring (Python)

Responsibilities Identified

The process_orders() function is doing too many things:

Validating inventory availability.
Checking stock quantity.
Validating customer details.
Calculating product prices.
Applying customer discounts.
Updating inventory.
Calculating shipping costs.
Calculating taxes.
Creating order summaries.
Tracking total revenue and errors.

This violates the Single Responsibility Principle, where a function should focus on one task.

# Suggested Smaller Functions

The function can be divided into:

validate_order() – Checks inventory, quantity, and customer existence.
calculate_price() – Calculates product price and discounts.
calculate_shipping() – Determines shipping charges.
calculate_tax() – Calculates tax amount.
update_inventory() – Reduces available stock.
create_order_summary() – Builds the result object.


# Example Refactored Structure

def process_orders(orders, inventory, customers):
    results = []
    errors = []
    revenue = 0

    for order in orders:
        error = validate_order(order, inventory, customers)

        if error:
            errors.append(error)
            continue

        price = calculate_price(order, inventory, customers)
        shipping = calculate_shipping(price, customers)
        tax = calculate_tax(price)

        update_inventory(order, inventory)

        final_price = price + shipping + tax
        revenue += final_price

        results.append(create_order_summary(order, price, shipping, tax, final_price))

    return {
        "processed_orders": results,
        "error_orders": errors,
        "total_revenue": revenue
    }


# Benefits of Refactoring
The code is more readable.
Functions can be unit tested separately.
Changes in the future are easier.
Insects are more readily detectable.


# What I Learned
Typically, long functions have several responsibilities.
Smaller functions make for easier maintenance.
Simple Functions Facilitate Good Unit Testing.

========================================================================================================================================================================

## Exercise 3: Code Duplication Detection (JavaScript)

Repeated Patterns Identified

The function repeats almost identical loops for:

Average calculations:
Average age
Average income
Average score
Highest value calculations:
Highest age
Highest income
Highest score

The only difference between these loops is the property being processed.

# Suggested Solution

Create reusable helper functions:

function calculateAverage(users, property) {
    let total = 0;

    for (const user of users) {
        total += user[property];
    }

    return total / users.length;
}

function findHighest(users, property) {
    let highest = users[0][property];

    for (const user of users) {
        if (user[property] > highest) {
            highest = user[property];
        }
    }

    return highest;
}

# The main function becomes simpler:

function calculateUserStatistics(userData) {
    return {
        age: {
            average: calculateAverage(userData, "age"),
            highest: findHighest(userData, "age")
        },
        income: {
            average: calculateAverage(userData, "income"),
            highest: findHighest(userData, "income")
        },
        score: {
            average: calculateAverage(userData, "score"),
            highest: findHighest(userData, "score")
        }
    };
}

# Benefits of Removing Duplication
Reduces the amount of repeated code.
Fixes and improvements only need to be made in one location.
The main function is easier to read.
The chances of introducing inconsistent logic are reduced.

##  Reflection Questions

1. Which prompting strategy was most useful? Why?
The Function Refactoring strategy was the most useful because it helped identify functions with too many responsibilities and showed how to split them into smaller, reusable functions.

2. What improvements did AI suggest that I might not have thought of?
AI identified issues such as poor naming conventions, code duplication, breaking large functions into smaller functions, and security risks like SQL injection.

3. Were there any suggestions I disagreed with? Why?
Most suggestions were useful. However, some refactoring changes should be evaluated based on the project size because creating too many small functions can make simple code harder to follow.

4. How could these prompts be adapted for my own codebase?
The prompts can be customized by including the programming language, framework, team coding standards, and project requirements to receive more relevant refactoring suggestions.

5. What safeguards should be used before applying AI refactoring to production code?
Ensure existing tests pass before making changes.
Add unit and integration tests for important functionality.
Review all AI-generated suggestions manually.
Perform code reviews with team members.
Test the refactored code in a development environment before deployment.