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

## Normalisering

> Normalisering innebär att man tar bort anomalier. En anomali är en "konstighet", eller ett felaktigt beteende för databasen. 
> Dessa är hot mot C från ACID (Consistency), dvs motsatsen till korrupt. Så vi skyddar vår data genom att ge den bättre förutsättningar att vara konsistent över åren, genom att ta bort anomalier i databasen. 
> 
> När anomalier är helt borta, är även redundant data borta.
> Vi siktar på tredje Normal Form ([3NF](https://en.wikipedia.org/wiki/Third_normal_form)). Ofta anser man att Boyce-Codd [BCNF](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form) räcker för att en databas ska vara tillräckligt normaliserad. 

### [1NF]()

> En relation är i första normal form, om och endst om, ingen attribut har relationer som element. Exempelvis Hobbies, Telefonnummer, Skola är exempel på relationer som finns i UNF tabellen.

### [2NF](https://en.wikipedia.org/wiki/Second_normal_form)

> Förklaras bättre på level-5.

### [3NF](https://en.wikipedia.org/wiki/Third_normal_form)

> Varje värde i databasen skall vara beroende enbart av primärnyckeln.

### [4NF](https://en.wikipedia.org/wiki/Fourth_normal_form)

### [5NF](https://en.wikipedia.org/wiki/Fifth_normal_form)

### [BCNF/3.5NF](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form)

> Ett tillägg, då Boyce och Codd insåg att 3NF inte tog bort alla anomalier, så de skapade 3.5NF.

> **_NOTE:_**  BCNF är den normalform som oftast är mål vid databas design.
