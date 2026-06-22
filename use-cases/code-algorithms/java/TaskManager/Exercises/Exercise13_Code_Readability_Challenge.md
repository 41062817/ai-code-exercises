##  Understanding the Original Code
The first approach is based on the Selection Sort algorithm. The algorithm is designed to repeatedly find the smallest element in the unsorted subarray and move it to its correct position to sort the array.

The code was correct, but there were a number of readability problems:

It didn't clarify what algorithm is being used.
No comments were provided that explain the algorithm procedure.
The names of variables were too short and lack of descriptive meaning: n, i, j, min_idx, temp.
No time and space complexity explanation was provided for the algorithm in the code.
It may be challenging for future developers to interpret the function of each section of the code.

A set of JUnit tests were run to verify the original functionality before changes were made.

##  Refactored Java Class

File: SortingAlgorithm.java

/**
 * Sorts an array of integers in ascending order using the Selection Sort algorithm.
 *
 * Selection Sort works by repeatedly finding the smallest element in the
 * unsorted part of the array and moving it to the correct position.
 *
 * Time Complexity:
 * - Best case: O(n²)
 * - Average case: O(n²)
 * - Worst case: O(n²)
 *
 * Space Complexity:
 * - O(1) because sorting is done in-place.
 */
public class SortingAlgorithm {

    public static void sortItems(int[] array) {

        // Total number of elements in the array
        int arrayLength = array.length;

        // Move through each position where the next smallest value belongs
        for (int currentPosition = 0; currentPosition < arrayLength - 1; currentPosition++) {

            // Assume the current position contains the smallest value
            int smallestIndex = currentPosition;

            // Search the remaining elements for a smaller value
            for (int searchIndex = currentPosition + 1;
                 searchIndex < arrayLength;
                 searchIndex++) {

                if (array[searchIndex] < array[smallestIndex]) {
                    smallestIndex = searchIndex;
                }
            }

            // Swap the current element with the smallest element found
            int temporaryValue = array[smallestIndex];
            array[smallestIndex] = array[currentPosition];
            array[currentPosition] = temporaryValue;
        }
    }
}


##   Unit Test Class

File: SortingAlgorithmTest.java

The existing JUnit tests were used to verify that the refactored code still produces the same results.

Example test:

import static org.junit.Assert.assertArrayEquals;
import org.junit.Test;

public class SortingAlgorithmTest {

    @Test
    public void testEmptyArray() {
        int[] emptyArray = {};
        int[] expected = {};

        SortingAlgorithm.sortItems(emptyArray);

        assertArrayEquals(
            "Empty array should remain unchanged",
            expected,
            emptyArray
        );
    }

    // The remaining tests from the provided exercise were kept unchanged:
    // testSingleElement()
    // testAlreadySorted()
    // testReverseSorted()
    // testRandomArray()
    // testDuplicateElements()
    // testNegativeNumbers()
    // testMixedNumbers()
}

The remaining provided tests were executed without modification.



##  JUnit Dependency

JUnit was added to the project to allow unit testing.

For Maven projects:

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>



##                    Running the Tests

The tests were executed using the IDE test runner (such as IntelliJ IDEA or Eclipse).

They can also be executed from the command line using Maven:

-mvn test


##  Test Results

After the refactoring, all existing unit tests passed successfully. This confirmed that the changes improved the readability and maintainability of the code without changing its external behavior.

The tests verified:

Empty arrays remain unchanged.
Single element arrays remain unchanged.
Already sorted arrays remain unchanged.
Reverse sorted arrays are sorted correctly.
Random arrays are sorted correctly.
Arrays containing duplicate values are sorted correctly.
Arrays containing negative numbers are sorted correctly.
Arrays containing both positive and negative numbers are sorted correctly.



## Readability Improvements Made
# Original Code	                                                  Improvement
No explanation of algorithm	                                      Added JavaDoc comments explaining Selection Sort
No complexity information	                                      Added time and space complexity
n	                                                              Renamed to arrayLength
i	                                                              Renamed to currentPosition
j	                                                              Renamed to searchIndex
min_idx	                                                          Renamed to smallestIndex
temp	                                                          Renamed to temporaryValue
No comments	                                                      Added comments describing each step
Harder to understand flow	                                      Improved structure and readability



## Reflection

-The refactored code is much better understood since it is using meaningful variable names and comments that explain the intent of each portion of the code.

-The most helpful modifications were adding documentation for the algorithm, and changing the names of variables that were not clear. The changes make it easier for new developers to understand the process of Selection Sort and maintain the code in the future.

-The AI-assisted review helped pinpoint readability issues, including a lack of documentation, unclear variable names, and missing complexity information. These are practices that could be used for future software projects.

-Developers should make sure that automated tests exist and that they are all executed after changes are made to production code to verify that the functionality remains unchanged before applying AI-generated refactoring to production code.