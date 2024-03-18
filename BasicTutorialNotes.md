# Basic/Int SQL Tutorial Notes

Playlist: https://www.youtube.com/watch?v=PyYgERKq25I&list=PLUaB-1hjhk8FE_XZ87vPPSfHqb6OcM0cF&index=4

**MISC**
- Capitalization doesn't matter when entering functions
- Query reruns everything, you need to comment out old stuff or open another query box to 

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
- SELECT `COUNT(LastName)` AS LastNAme Count FROM... returns the amount of unique entries and saves values to a seperate column
- SELECT `MAX(Salary)`
  * Also applies for min and avg
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



https://www.youtube.com/watch?v=lYKkro6rKm0&list=PLUaB-1hjhk8FE_XZ87vPPSfHqb6OcM0cF&index=8
















**Unions**
**Case Statements**
**Updating/Deleting Data**
**Partition By**
**Data Types**
**Aliasing**
**Creating Views**
**Having vs Group By Statement**
**GETDATE()**
**Primary vs Foreign Key**










