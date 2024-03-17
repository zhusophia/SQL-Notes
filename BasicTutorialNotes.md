# Basic SQL Tutorial Notes

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
