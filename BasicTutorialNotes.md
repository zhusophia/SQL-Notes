# Basic/Int SQL Tutorial Notes

Playlist: https://www.youtube.com/watch?v=PyYgERKq25I&list=PLUaB-1hjhk8FE_XZ87vPPSfHqb6OcM0cF&index=4

**MISC**
- Capitalization doesn't matter when entering functions
- Query reruns everything, you need to comment out old stuff or open another query box to not have it run everything. You can highlight and it will exclusively run the highlighted sections

**CREATE TABLE** 
```
CREATE TABLE EmployeeDemographics(
EmployeeID int, 
FirstName varchar(50))
```
**INSERT INTO**
- Insert values into a table
- Should make sure to match how many columns the table already has
```
Insert INTO EmployeeDemographics VALUES
(1002, 'Receptionist', 36000), 
(1003, 'Salesman', 63000))
```

**SELECT**
- Grab a column/row
```
SELECT FirstName, LastName FROM EmployeeDemographics
```
- `*` means (grab) everything
- SELECT `TOP 5 *` for top 5 rows
- SELECT `DISTINCT(EmployeeID)` FROM... will only take the distinct rows
- SELECT `COUNT(LastName)` AS LastName Count FROM... returns the amount of unique entries and saves values to a seperate column
- SELECT `MAX(Salary)`
  * Also applies for min and avg
  * Need to remember to put `GROUP BY` whatever you're counting/aggregating functioning (combining multiple values into one) to make it into a new row(?)
- If you want to select something from another database, you use
```
SELECT * FROM [SQL TUTORIAL].dbo.EmployeeSalary
```

**WHERE**
- Extension of the `select` function.
```
SELECT FirstName, LastName
FROM EmployeeDemographics
WHERE FirstName = 'Jim'
```
- <> means does not equal
- <, >, And, Or, Like, is Null, is Not Null, In
- `WHERE LastName LIKE 'S%'` means grab any lastname that starts with S
  - '%S%' -> There's an S anywhere (including first and last letter) in the last name
  - 'S%ott%c%' means there's an S at the beginning, there's an ott somewhere in it, but OTT must PROCEED a C
- `Where FirstName IN ('Jim', 'Michael') is jsut another way of grabbing multiple entries (shortens and and or commands)

**GROUP BY**
- Extension of SELECT, groups categories together
- Can GROUP BY multiple categoreis at once, e.g. `GROUP BY Gender, Age` 
```
SELECT Gender
FROM EmployeeDemographics
GROUP BY Gender
```

**ORDER BY**
- Extension of SELECT, orders automatically in ASC order.
```
SELECT *
FROM EmployeeDemographics
ORDER BY Age DESC, Gender
```
- `ORDER BY CountGender DESC` changes it to descending
- `ORDER BY 4 DESC, 5 DESC` (replacing column name with column number, starts at 1) does the same thing


**Joins**
[See this Venn Diagram image](https://www.dofactory.com/img/sql/sql-joins.png)
- Note that EmployeeDemographics is the left table, EmployeeSalary is the right table

*Inner Join*
- Combine two tables by using a similar column. Only keeps what is similar between the specified column of two tables
- Automatic default for `JOIN`
```
SELECT *
FROM SQLTutorial.dbo.EmployeeDemographics 
JOIN SQLTutorial.dbo.EmployeeSalary           #call the table you want to join 
 ON EmployeeDegraphics.EmployeeID = EmployeeSalary.EmployeeID          #call the column you want to join
```

*Full Outer Join*
- Shows everything from Table A and Table B, regardless if it has a match based on what we've specified to join them on
```
SELECT *
FROM SQLTutorial.dbo.EmployeeDemographics 
Full Outer JOIN SQLTutorial.dbo.EmployeeSalary           #call the table you want to join 
 ON EmployeeDegraphics.EmployeeID = EmployeeSalary.EmployeeID          #call the column you want to join
```

Variations 
- Left Outer Join: Everything from the left table (first table) and everything overlapping.
- Right Outer Join: Everything from the left table (first table) and everything overlapping.

You need to specific which Employee ID you want to grab things from 

```
SELECT EmployeeDemographics.EmployeeID, FirstName, LastName, JobTitle, Salary
FROM SQLTutorial.dbo.EmployeeDemographics 
Full Outer JOIN SQLTutorial.dbo.EmployeeSalary           #call the table you want to join 
 ON EmployeeDegraphics.EmployeeID = EmployeeSalary.EmployeeID          #call the column you want to join
```
- Prevents overlap for multiple columns by only selecting certain columns

What you select from, and the type of join you do, you're going to get different things 
- Select Employee Salary, Left outer join
 - We grab every single column in the the left table -> Even if they have no Employee ID or Salary in the right table
 - But, since we are calling the Employee ID from the RIGHT table (where some of the left table employees don't exist or only have their Employee ID on the left table), the EmployeeID field can be NULL since the employee only exists on the left table. 

**Unions**
- Combine two tables to create one output, without a need for a similar column (that you need for `join`).
```
SELECT *
FROM EmployeeDemographics
UNION
SELECT *
FROM WareHouseEmployeeDemographics
```
- Super useful if two tables have the same columns and you just need to merge them together
- Removes duplicates. If you want duplicates, you should replace `UNION` with `UNION ALL`

You can also union two tables that do not have the same columns. They just need the same column TYPES
```
SELECT EmployeeID, FirstName, Age
FROM EmployeeDemographics
UNION
SELECT EmployeeID, JobTitle, Salary
FROM EmployeeSalary
ORDER BY EmployeeID
```
- FirstName and JobTitle entries get merged together and upt in a column called FirstName, since they are both varchar types

**Case Statements**
- If statements
```
SELECT FirstName, LastName, Age,
CASE
    WHEN Age > 30 THEN 'Old'
    WHEN Age BETWEEN 27 AND 30 THEN 'Young'
    ELSE 'Baby'
END AS NameAge    #gives the column a name 
FROM EmployeeDemographics
WHERE Age is NOT NULL
ORDER BY Age
```
- If someone satisifies more than one `WHEN` statement, they will automatically be categorized into the first applicable `WHEN` statement. 

**HAVING CLAUSE**
- Can not use an aggregate function (max, min, avg, etc.) in a `WHERE` function. Can only use it in a `HAVING` function.
- Basically just only shows data if a certain condition is met in the
```
SELECT JobTitle, COUNT(JobTitle)
FROM EmployeeDemographics
JOIN Employee Salar
    ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
GROUP BY JobTtitle
HAVING AVG(Salary) > 45000    #Only show salaries greater than 45000
ORDER BY AVG(Salary)
```
- Needs to go after the `Group By` statement (Can't look at aggregated information before it's been aggregated 

**Updating/Deleting Data**
```
SELECT *
FROM EmployeeDemographics
UPDATE EmployeeDemographics   #select table
SET EmployeeID = 1012
WHERE FirstName = 'Holly' AND Lastname = 'Flax'

DELETE FROM EmployeeDemographics
WHERE EmployeeID = 1005
```
- You can not 'undo' a delete 

**Aliasing**
- Tempararily changing a column name to improve readability

Examples:
```
SELECT FirstName + ' ' + LastName AS FullName
FROM EmployeeDemographics
```
```
SELECT Demo.EmployeeID, Sal.Salary
FROM SQLTutorial.dbo.EmployeeDemographics AS Demo
JOIN SQLTutorial.dbo.EmployeeSalary AS Sal
    On Demo.EmployeeID = Sal.EmployeeID
```  

**Partition By**
- Similar to `GROUP BY` -> reduces number of rows by rolling them up and calculating sums or avgs of group
- `Partition By` -> Divides data into partitions and changes how the function is calculated, doesn't change the number of rows

```
SELECT Firstname, LastName, Gender, Salary, COUNT(Gender) OVER (Partition BY Gender) as TotalGender
FROM EmployeeDemographics dem
JOIN EmployeeSalary sal
    On dem.EmployeeID = sal.EmployeeID
```
- Count the number of Females and Males, for each female, put the number of females in the column TotalGender. For each male, put the number of males in the column TotalGender

**String Manipulation**

_TRIM_
- Removes spaces at from and back. Also left and right trim (which only trims left or right side)
```
SELECT EmployeeID, TRIM(EmployeeID) as IDTRIM
FROM EmployeeErrors
```

_REPLACE_
- Replace a certain substring with another substring of your choice
```
Select LastName, REPLACE(LastName, '- Fired', '') as LastNameFixed
FROM EmployeeErrors
```

_SUBSTRING_
- Take a specific section of the string. Starts at 1
```
SELECT SUBSTRING(FirstName, 1, 3 #grabs string from char 1 to char 3 (ends up being 3 letters long)
FROM EmployeeErrors 
```
- You can use it for fuzzy matching - take the first three characters using `SUBSTRING` and then you can use `JOIN` to match them (Recall that `JOIN` only works if the strings are the same).

_UPPER/LOWER_
- Changes it all to be all Upper or Lower case.
```
Select firstname, LOWER(firstname)
from EmployeeErrors

Select Firstname, UPPER(FirstName)
from EmployeeErrors
```

Other Potential Topics: 
**Data Types**
**Creating Views**
**Having vs Group By Statement**
**GETDATE()**
**Primary vs Foreign Key**

https://www.youtube.com/watch?v=Twusw__OzA8&list=PLUaB-1hjhk8FE_XZ87vPPSfHqb6OcM0cF&index=9








