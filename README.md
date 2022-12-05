# edu-intro-mysql

> Normalisera Student

```sql
DROP TABLE IF EXISTS Student;

CREATE TABLE Student (
    Id INT NOT NULL,
    Name VARCHAR(26) NOT NULL
)  ENGINE=INNODB;

INSERT INTO Student (Id, Name) SELECT Id, Name FROM UNF;

select * from student;
SELECT DISTINCT * FROM Student;

```

## Hitta dubletter

```sql
SELECT Id, Name, Count(*) as Count FROM Student GROUP BY Id, Name HAVING Count > 1;

SELECT * FROM Student WHERE ID IN (5,13,27);
```

## Lägg krav på att id måste vara unik på Student

```sql
DROP TABLE IF EXISTS Student;

CREATE TABLE Student (
    Id INT NOT NULL,
    Name VARCHAR(26) NOT NULL,
    CONSTRAINT constraint_unique_StudentId UNIQUE (Id)
)  ENGINE=INNODB;

SHOW CREATE TABLE Student;

SELECT COLUMN_NAME, CONSTRAINT_NAME, REFERENCED_COLUMN_NAME, REFERENCED_TABLE_NAME
FROM information_schema.KEY_COLUMN_USAGE
WHERE TABLE_NAME = 'Student';

INSERT INTO Student (Id, Name) SELECT Id, Name FROM UNF;
```

## Lägg primärnyckel på Student

```sql
ALTER TABLE Student ADD PRIMARY KEY(Id);

DROP TABLE IF EXISTS Student;
CREATE TABLE Student (
    Id INT NOT NULL,
    Name VARCHAR(26) NOT NULL,
    CONSTRAINT PRIMARY KEY (Id)
)  ENGINE=INNODB;

DESC Student;
```

## Lägg till ny data till Student

```
INSERT INTO Student(Name) VALUES('Nisse Karlsson');
SELECT max(ID) FROM Student);

ALTER TABLE Student MODIFY COLUMN Id Int AUTO_INCREMENT;

DROP TABLE IF EXISTS Student;
CREATE TABLE Student (
    Id INT NOT NULL AUTO_INCREMENT,
    Name VARCHAR(26) NOT NULL,
    CONSTRAINT PRIMARY KEY (Id)
)  ENGINE=INNODB;
```

## Dela upp Förnamne och Efternamn

```sql
SELECT SUBSTRING_INDEX(Name, ' ', 1) AS FirstName, SUBSTRING_INDEX(Name, ' ', -1) AS LastName FROM UNF;
```

###
DROP TABLE IF EXISTS Student;
CREATE Table Student AS 
SELECT DISTINCT 
ID, SUBSTRING_INDEX(Name, ' ', 1) AS FirstName, SUBSTRING_INDEX(Name, ' ', -1) AS LastName 
FROM UNF;

ALTER TABLE Student ADD PRIMARY KEY(Id);

ALTER TABLE Student MODIFY COLUMN Id Int AUTO_INCREMENT;
```
