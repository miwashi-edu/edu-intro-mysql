# edu-intro-mysql

## Beskrivning

> Vi normaliserar student. Vi kombinerar DQL och DDL för att kunna få fatt på rätt data och skapa nödvändiga tabeller.
> Vi använder oss av INSERT INTO, för att fylla tabellen.

```mermaid
erDiagram
    Student {
        int StudentId
        string Name
    }    
```

> **_NOTE:_**  Läs mer om normalisering i slutet.

## Instruktioner

### I container-with-mysql

### Analys - Hitta dubletter

> Ofta i analysen försöker vi lista ut så mycket vi kan om datat. Det vi ser är att skola är vad som skapar dubletter. Samma person går i två skolor och blir därför två rader. Sedan ser vi att stad samvarierar med skola, och vi kan anta att stad är ett attribut till Skola och eventuellt ingen egen entitet.

```sql
SELECT Id, Name, Count(*) as Count FROM UNF GROUP BY Id, Name HAVING Count > 1;

SELECT * FROM UNF WHERE ID IN (5,13,27);
```

> **_NOTE:_**  När vi grupperar (group by) tillåter inte SQL längre att vi skriver WHERE. Då måste vi använda HAVING istället.


#### Försök 1, utan kontroll för unikhet

```sql
DROP TABLE IF EXISTS Student;

CREATE TABLE Student (
    StudentId INT NOT NULL,
    Name VARCHAR(256) NOT NULL,
)  ENGINE=INNODB;

INSERT INTO Student (StudentId, Name) SELECT Id, Name FROM UNF;

/* Hitta dubletter genom att jämföra fråga med DISINCT mot fråga utan. */
SELECT * FROM Student;
SELECT DISTINCT * FROM Student;
```

#### Försök 2, med kontroll för unikhet

```sql
DROP TABLE IF EXISTS Student;

/* Vi lägger till en Constraint */
CREATE TABLE Student (
    StudentId INT NOT NULL,
    Name VARCHAR(26) NOT NULL,
    CONSTRAINT constraint_unique_StudentId UNIQUE (StudentId)
)  ENGINE=INNODB;

/* Visa hur tabellen blev skapad */
SHOW CREATE TABLE Student;

/* Detta kastar ERROR 1062 Duplicate entry */
INSERT INTO Student (StudentId, Name) SELECT Id, Name FROM UNF;

/* Men utan dubletter fungerar det */
INSERT INTO Student (StudentId, Name) SELECT DISTINCT Id, Name FROM UNF;
```

#### Försök 3, med primärnyckel

```sql
ALTER TABLE Student ADD PRIMARY KEY(Id);

DROP TABLE IF EXISTS Student;
CREATE TABLE Student (
    StudentId INT NOT NULL,
    Name VARCHAR(26) NOT NULL,
    CONSTRAINT PRIMARY KEY (Id)
)  ENGINE=INNODB;

DESC Student;
```

#### Dela upp Förnamne och Efternamn

> Man brukar ofta dela upp förnamn och efternamn, då det ofta är svårt att läsa utifrån namnet vilket som är förnamn och vilket som är efternamn.

```sql
SELECT SUBSTRING_INDEX(Name, ' ', 1) AS FirstName, SUBSTRING_INDEX(Name, ' ', -1) AS LastName FROM UNF;

DROP TABLE IF EXISTS Student;

CREATE Table Student AS 
SELECT DISTINCT 
ID AS StudentId, SUBSTRING_INDEX(Name, ' ', 1) AS FirstName, SUBSTRING_INDEX(Name, ' ', -1) AS LastName 
FROM UNF;

ALTER TABLE Student ADD PRIMARY KEY(Id);

ALTER TABLE Student MODIFY COLUMN Id Int AUTO_INCREMENT;
```

## Normalisering

> Normalisering innebär att man tar bort anomalier. En anomali är en "konstighet", eller ett felaktigt beteende för databasen. 
> Dessa är hot mot C från ACID (Consistency), dvs motsatsen till korrupt. Så vi skyddar vår data genom att ge den bättre förutsättningar att vara konsistent över åren, genom att ta bort anomalier i databasen. 
> 
> När anomalier är helt borta, är även redundant data borta.
> Vi siktar på tredje Normal Form ([3NF](https://en.wikipedia.org/wiki/Third_normal_form)). Ofta anser man att Boyce-Codd [BCNF](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form) räcker för att en databas ska vara tillräckligt normaliserad. 

## 1NF

## 2NF

## 3NF

## 4NF

## 5NF
