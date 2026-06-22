## Data Processing Pipeline with Error Handling (Java)

# Identifying the Distinct Responsibilities

The original processCustomerData() function violates the Single Responsibility Principle because it performs many unrelated tasks. The responsibilities identified are:

1. Input validation
- checks whether raw data exists before processing.
2. Loading existing customer data
- Loads emails and phone numbers from the database for deduplication.
3. Source-specific preprocessing
- Handles different data sources such as CSV, API, and manual input.
4. Customer field validation
- Validates email, name, phone number, address, and date of birth.
5. Duplicate checking
- Detects duplicate emails and phone numbers against current records and the database.
6. Custom validation
- Runs additional validation provided through CustomerProcessingOptions.
7. Error handling
- Tracks validation failures and unexpected processing exceptions.
8. Database operations
- Converts valid records into customer entities and saves them.
9. Report generation
- Creates the final processing summary containing statistics and errors.


## Decomposition Plan

The large function can be divided into smaller helper functions with clear responsibilities.

# New Function	                              Responsibility
validateInputData()	                          Checks whether input data is available
loadExistingCustomers()	                      Loads existing emails and phone numbers
preprocessRecord()	                          Applies CSV, API, or manual preprocessing
validateEmail()	                              Validates and normalizes email addresses
validateNameFields()	                      Validates first and last names
validatePhoneNumber()	                      Validates and formats phone numbers
validateAddress()	                          Processes and validates addresses
validateDateOfBirth()	                      Checks date format and age limits
checkDuplicates()	                          Detects duplicate emails and phone numbers
applyCustomValidation()	                      Executes user-provided validations
saveValidRecords()	                          Converts and stores customers in database
generateProcessingReport()	                  Creates the final processing summary



##  Refactored Main Function

The main function becomes much smaller and easier to understand.

public Map<String, Object> processCustomerData(
        List<Map<String, Object>> rawData,
        String source,
        CustomerProcessingOptions options) {

    validateInputData(rawData);

    ProcessingContext context = new ProcessingContext();

    if (options.isPerformDeduplication()) {
        loadExistingCustomers(context);
    }

    for (Map<String, Object> record : rawData) {

        Map<String, Object> processedRecord =
                preprocessRecord(record, source);

        ValidationResult validation =
                validateRecord(processedRecord, context, options);

        if (validation.isValid()) {
            context.addValidRecord(processedRecord);
        } else {
            context.addInvalidRecord(
                processedRecord,
                validation.getErrors()
            );
        }
    }

    if (options.isSaveToDatabase()) {
        saveValidRecords(context.getValidRecords(), source);
    }

    return generateProcessingReport(context, rawData.size());
}



##  Extracted Helper Functions

Input Validation
private void validateInputData(List<Map<String, Object>> data) {
    if (data == null || data.isEmpty()) {
        throw new IllegalArgumentException(
            "No data provided for processing"
        );
    }
}



## Source Preprocessing

private Map<String, Object> preprocessRecord(
        Map<String, Object> record,
        String source) {

    switch (source) {
        case "csv":
            return preprocessCsvRecord(record);

        case "api":
            return preprocessApiRecord(record);

        case "manual":
            return preprocessManualRecord(record);

        default:
            return record;
    }
}


## Email Validation

private boolean validateEmail(String email) {
    return email != null &&
           email.matches(
            "^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,6}$"
           );
}



## Saving Records

private void saveValidRecords(
        List<Map<String, Object>> records,
        String source) {

    List<Customer> customers = new ArrayList<>();

    for (Map<String, Object> record : records) {
        Customer customer = mapToCustomerEntity(record);
        customer.setDataSource(source);
        customer.setCreatedAt(new Date());
        customers.add(customer);
    }

    customerRepository.saveAll(customers);
}



##  Testing the Refactoring

Existing tests should be run first prior to applying the refactoring to capture the behavior of the system as the tests are applied.

Following the changes, tests should ensure that:
# Unit Tests:

If the data is null or empty, input validation throws back an error.
All records are processed correctly, including CSV, API, or manual records.
If an email address is not valid, it is rejected.
Numbers that are not correct are not accepted.
Duplicate customer records are detected.
Address formatting is correct.
Age checks are validated by Date of birth validation.
Custom validators will provide expected errors.
Customer entities are saved correctly when saving to the database.

# Integration Tests:

All of the customer details is dealt with properly.
The valid records are stored in the database.
Records that do not have a valid version number are reported as such.
Processing statistics are equal to original implementation.
Error handling is effective in case of a database failure.


## Benefits of the Refactoring

The main function now explicitly states the processing workflow to promote readability.
Single responsibility— each helper method does one thing.
Smaller methods can be tested independently, thus making it easier to test.
Easier to maintain as validation and/or preprocessing concerns only one area.
Less complexity – Conditional statements that are nested are eliminated.
Improved error handling due to failures being limited to certain stages of the process.
Better reusability as other customer-processing features can utilise helper functions.

## Reflection

In this exercise, you saw how AI can help you discover large functions with too many responsibilities and tame them into small, more focused functions. The primary takeaway is that the code refactoring should be done without altering the functionality being exposed to the outside world. With the clear helper methods, the code will be easy to read, maintain, test and extend later when developing.