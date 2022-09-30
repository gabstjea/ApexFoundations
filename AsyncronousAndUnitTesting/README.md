# Asyncronous Apex and Unit Testing

## Unit Testing
* Syntax
  * `@isTest static void testName() {}`
* For code validation:
  * System.assert(condition, msg)
    * Asserts that a condition must be true
  * System.assertEquals(expected, actual, msg)
  * System.assertNotEquals(expected, actual, msg)
* Apex Unit tests can test Apex triggers.
  * See [uTest1_TestDML](./uTest1_TestDML)
  * DML operations performed in a unit test are automatically deleted
