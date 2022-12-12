# edu-intro-mysql

> Normalisering av School, Student School är en så kallad många till många relation. Detta kräver en kopplingstabell.

## Undersök data

```
SELECT 
Count(DISTINCT Name, City), 
Count(Distinct Name), 
Count(DISTINCT School, City), 
Count(DISTINCT School) 
FROM UNF;
```

## Skapa School

```sql
DROP TABLE IF EXISTS School;
CREATE TABLE School AS SELECT DISTINCT 0 As SchoolId, School As Name, City FROM UNF;

SET @id = 0;
UPDATE School SET SchoolId =  (SELECT @id := @id + 1);

ALTER TABLE School ADD PRIMARY KEY(SchoolId);
```

## Skapa kopplingstabell

```sql
CREATE TABLE StudentSchool AS SELECT DISTINCT UNF.Id AS StudentId, School.SchoolId
FROM UNF INNER JOIN School ON UNF.School = School.Name;
ALTER TABLE StudentSchool MODIFY COLUMN StudentId INT;
ALTER TABLE StudentSchool MODIFY COLUMN SchoolId INT;
ALTER TABLE StudentSchool ADD PRIMARY KEY(StudentId, SchoolId);

SELECT StudentId, FirstName, LastName FROM Student
JOIN StudentSchool USING (StudentId);

SELECT StudentId, FirstName, LastName, Name, City FROM Student
JOIN StudentSchool USING (StudentId) 
JOIN School USING (SchoolId);
```
