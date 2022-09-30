# Asyncronous Apex and Unit Testing

## Unit Testing
* Syntax
  * `@isTest static void testName() {}`
* For code validation:
  * System.assert(condition, msg)
    * Asserts that a condition must be true
  * System.assertEquals(expected, actual, msg)
  * System.assertNotEquals(expected, actual, msg)
* Apex Unit tests can test Apex triggers and DML operations
  * Invoking `.addError('msg')` on an object prevents DML operations from being performed on it
  * See [uTest1_TestDML](./uTest1_TestDML)
  * DML operations performed in a unit test are automatically deleted
