# DML, SOQL, and SOSL
This repository contains information on how to utilize the Data Manipulation Language (DML), Salesforce Object Query Language (SOQL), and Salesforce
Object Search Language (SOSL) which are all critically components of the Apex language. DML is used for managing data in sales force by the use of 
commands such as upsert, delete, insert, and etc, while the latter two are used for querying objects.<br /><br />

Within a query, avoid the followinf within a WHERE clause: filtering based on null rows, using the not equals and comparison operators, and using 
the wild card operator. These operations are taxing on resources provided by salesforce which may cause issues with governer limits (the set amount
of allocated resources that Salesforce users can use so that resources in Salesforce's multitenated environment can be managed equally).<br /><br />

<a href="#user-content-salesforce-object-query-language-soql" id="Salesforce-Object-Query-Language-soql"> Salesforce Object Query Language</a><br />
<a href="#salesforce-object-search-language-sosl" id="salesforce-object-search-language-sosl"> Salesforce Object Search Language</a>

# Salesforce Object Query Language (SOQL)

* Apex has direct access to Salesforce records by the use of SOQL.
* A record is a instance of a sObject or custom object.
* A field is a instance attribute of a sObject or custom object.
* Place query statement in square brackets to use within Apex.
* The query editior console can be used to test queries.
* [sObject Referece Document](https://developer.salesforce.com/docs/atlas.en-us.224.0.object_reference.meta/object_reference/sforce_api_objects_contact.htm)

## SOQL Syntax
* Required 
  * SELECT - The field to query 
    * The ID of an sOBject is automatically queried so no need to speficy it
  * FROM - The sObject to query <br />
* Optional
  * WHERE - The search criteria
    * Omitting WHERE Queries all records
    * Supports logic operators (>, <, >=. <=, AND, OR, etc)
  * Can be used with AND and OR logic operators
  * LIKE - Regex mathcing. Used with WHERE
    * Uses the `%` and `?` wildcard
    * Alternative to `=` in WHERE clause
  * NOT LIKE
    * Alternative to `!=` in WHERE claiuse
  * ORDER BY `Field_Name` ASC
  * ORDER BY `Field_Name` DSC
  * LIMIT `n`
  * GROUP BY
  * IN - Used within WHERE to return objects that match any element in a comma separated list
    * So instead of using `WHERE Name='Gabriel'`, you can use `WHERE Name IN ('Gabriel')` to filter for objects that contain Gabriel in their name field. The
    only difference is that the latter can filter for multiple names other than Gabriel by using a comma for each name. Ex `WHERE Name IN ('Gabriel', 'Jack')`
    or `WHERE Name IN :nameList`
* Aggregates
  * AVG() - Returns the average value of a numeric field
  * COUNT() - Obtains a receord count
  * COUNT(fieldName) -
  * COUNT_DISTINCT()
  * MIN() - Returns the minium value of the field
  * MAX) - Returns the maximum value of the field
  * SUM() - Retruns the total sum of a numeric field
* To specify a variable
  * Using the `:` operator before the variable name
* Wildcard operators
  * `%` and `?`

## Query Use Cases
### Querying Multiple Fields
`SELECT FirstName, LastName FROM Contact`
  * Queries all Contact records first and last name information <br />

### Querying By Order
`SELECT FirstName FROM Contact ORDER BY FirstName`
  * Orders the first name of Contact records in ascending order <br />

### Querying With Logical Operators
`SELECT Name FROM Account WHERE (Name='Intel Computing' AND NumberOfEmployees > 25)` <br />
`SELECT FirstName, LastName FROM Contact WHERE (FirstName='Gabriel' OR FirstName='Jack'`

### Querying With LIKE statement
`SELECT Name FROM Account WHERE Name LIKE 'Google%'`
  * Obtains all Account names starting with Google <br />

### Querying a field with a variable
`SELECT FirstName FROM Contact WHERE FirstName =: firstNameVariable`
  * The `:` operator infront of a `=` operator to denote a varibale to be used in a query
  * see [ce01_SOQL](./ce01_SOQLquery)
  
### Writing Child-to-Parent Query
`SELECT FirstName, LastName, Account.Name FROM Contact`
  * Queries the first name, last name, and the Account name (Account is the parent) of the Contact.
  * The Dot notation can access up to five generation of parent infromation.
  * See [dml06_QueryParentRelatedRecord](./dml06_QueryParentRelatedRecord) <br />

### Writing Parent-to-Child Query (Nested Query)
`SELECT Name, (SELECT FirstName, LastName FROM Contacts) FROM Account`
  * The name of the child in the query must be plural.
  * Query all the names of every account record and display the related Contact recod's first and last name.
  * See [soql1_QueryChildRelatedRecord](./soql1_QueryChildRelatedRecord). <br />

# Salesforce Object Search Language (SOSL)
* Used for searching for objects based on a fields name, phone, email, and side bar.
* Use SOSL over SOQL if you don't know the exact fields that your data resides within partciular objects.
* Dev Reference [doc](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_sosl_syntax.htm?_ga=2.82219286.1016873653.1663593901-1334770197.1660755932)

## SOSL Syntax
* FIND - The feild to look for
* IN - Specifies the field type to look for
  * ALL FIELDS
  * NAME FIELDS
  * PHONE FIELDS
  * EMAIL FIELDS
* RETURNING - The object to return and specified fields to populate/query
  * If no field is specified, only the ID of the object will be queried if it matches the FIND field
* Wildcard operators
  * `%` and `?`

## SOSL Use Cases
### Finding an Object based on a FIND criteria
`FIND {"grand"} IN ALL FIELDS RETURNING Account(Name), Contact(FirstName, LastName)`
* Returns an Account and Contact object if it has a field named grand. The name field of the Account object is queried as well as the 
* first and last name of the Contact. <br />

### Finding an Object with a wild card
`FIND {"St Jean%"} IN NAME FIELDS RETURNING Contact(), Account()
* Returns a Contact and Account object if the name field has string beginning with St. Jean. The ID of the object is populated. <br />

### Using SOSL with a variable
`FIND :variable_Name IN ALL FIELDS RETURNING Contact()
