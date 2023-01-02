# edu-intro-mysql

> Vi normaliserar Hobbies. Hobbies är en relation i relation, och den bryter mot 1NF.

## Analys

> I det här läget visar analysen att filen är väl organiserad. Hade analysen visat att det finns "smuts", hade vi använt UPDATE först för att putsa data.

```sql
/* Kontrollera om det finns mellanslag före eller efter */
SELECT trim(HOBBIES) FROM UNF WHERE trim(HOBBIES) != HOBBIES;

SELECT COUNT(*) FROM UNF 
WHERE trim(Hobbies) LIKE ",%"  OR trim(Hobbies) LIKE "%,";

/* Kontrollera om det finns data som slutar eller börjar på "," */
/* Om det finns kan det påverka när vi splittar raden på komma sedan */
SELECT COUNT(*) FROM UNF 
WHERE trim(Hobbies) LIKE ",%"  OR trim(Hobbies) LIKE "%,";

/* Räkna komma tecken */
SELECT Hobbies, length(Hobbies) - length(replace(Hobbies, ',', '')) AS Count 
FROM UNF;

/* Kontrollera max antal Hobbies */
SELECT max(length(Hobbies) - length(replace(Hobbies, ',', ''))) AS Max 
FROM UNF;
```

## Alla första fält

> Villkor för när vi vill ha första fältet från hobbies.

```sql
SELECT Id, "We want first from all!", HOBBIES FROM UNF
WHERE HOBBIES != "";
```

## Alla mittersta fält

> Villkor för när vi vill ha mittersta fältet från hobbies. I det här fallet har jag räknat komma tecken, genom att ersätta dom med ingenting och mäta skillnaden i längden på strängen. Försvinner två komma tecken, är det tre fält och det finns en mittersta.


```sql
SELECT Id, "We want middle when three!", HOBBIES FROM UNF
WHERE (LENGTH(Hobbies) - LENGTH(REPLACE(Hobbies, ',', ''))=2);
```

## Alla sista fält

> Villkor för när vi vill ha sista fältet från hobbies. Alltid när vi har ett komma tecken, kommer det att finnas ett "sista" fält.

```sql
SELECT Id, "We want last when more than one!", HOBBIES FROM UNF
WHERE (LENGTH(Hobbies) - LENGTH(REPLACE(Hobbies, ',', ''))>=1);
```

## Alternativa WHERE satser

> Exprerimentera med dessa WHERE satser.

```sql
where Hobbies not like "%,%,%" and Hobbies like "%,%";
where Hobbies not like "%,%";
where Hobbies like '%,';
```

## Do it

```sql

First Field: trim(substring_index(Hobbies, ",", 1))
Middle Field: trim(substring_index(substring_index(Hobbies, ",", -2),"," ,1))
Last Field: trim(substring_index(Hobbies, ",", -1))

SELECT Id as StudentId, trim(SUBSTRING_INDEX(Hobbies, ",", 1)) AS Hobby FROM UNF
WHERE HOBBIES != ""
UNION SELECT Id as StudentId, trim(substring_index(substring_index(Hobbies, ",", -2),"," ,1)) FROM UNF
WHERE HOBBIES != ""
UNION SELECT Id as StudentId, trim(substring_index(Hobbies, ",", -1)) FROM UNF
WHERE HOBBIES != "";

```

## Do it with subquery

> Subquery är när du ändrar FROM satsen till att peka på en fråga istället för tabell eller vy. Typ SELECT ... FROM (SELECT ... )

```sql
SELECT Id as StudentId, trim(substring_index(Hobbies, ",", -1)) AS Hobby 
FROM (SELECT * FROM UNF WHERE HOBBIES != "") AS UNF2
UNION SELECT Id as StudentId, trim(substring_index(substring_index(Hobbies, ",", -2),"," ,1))
FROM (SELECT * FROM UNF WHERE HOBBIES != "") AS UNF2
UNION SELECT Id as StudentId, trim(substring_index(Hobbies, ",", -1))
FROM (SELECT * FROM UNF WHERE HOBBIES != "") AS UNF2;
```

## Efteranalys

> Så snart vi har en fråga, kan vi spara den som en VY, eller så använder vi frågan för att göra en subquery av den. Istället för att spara som VY (vilket är enklare, väljer jag att gå vidare med subquerys, vilket är lika enkelt om du har tungan rätt i mun när du copy pejstar.

```sql
/* Skapa hjälp-tabellen Hobby */

DROP TABLE IF EXISTS Hobbies;
CREATE TABLE Hobbies (
    HobbyId INT NOT NULL AUTO_INCREMENT,
    Name VARCHAR(255) NOT NULL,
    CONSTRAINT PRIMARY KEY (HobbyId)
)  ENGINE=INNODB;


INSERT INTO Hobbies(Name)
SELECT DISTINCT Hobby FROM (
  SELECT Id as StudentId, trim(SUBSTRING_INDEX(Hobbies, ",", 1)) AS Hobby FROM UNF
  WHERE HOBBIES != ""
  UNION SELECT Id as StudentId, trim(substring_index(substring_index(Hobbies, ",", -2),"," ,1)) FROM UNF
  WHERE HOBBIES != ""
  UNION SELECT Id as StudentId, trim(substring_index(Hobbies, ",", -1)) FROM UNF
  WHERE HOBBIES != ""
) AS Hobbies2;

/* Här gäller tungan rätt i copy pejstandet */
DROP TABLE IF EXISTS Hobby;
CREATE TABLE Hobby (
    StudentId INT NOT NULL,
    HobbyId INT NOT NULL,
    CONSTRAINT PRIMARY KEY (StudentId, HobbyId)
)  ENGINE=INNODB;

INSERT INTO Hobby(StudentId, HobbyId)
SELECT DISTINCT StudentId, HobbyId FROM (
  SELECT Id as StudentId, trim(SUBSTRING_INDEX(Hobbies, ",", 1)) AS Hobby FROM UNF
  WHERE HOBBIES != ""
  UNION SELECT Id as StudentId, trim(substring_index(substring_index(Hobbies, ",", -2),"," ,1)) FROM UNF
  WHERE HOBBIES != ""
  UNION SELECT Id as StudentId, trim(substring_index(Hobbies, ",", -1)) FROM UNF
  WHERE HOBBIES != ""
) AS Hobbies2 INNER JOIN Hobbies ON Hobbies2.Hobby = Hobbies.Name;

DROP VIEW IF EXISTS HobbiesList;
CREATE VIEW HobbiesList AS SELECT StudentId, group_concat(Name) FROM Hobby JOIN Hobbies USING (HobbyId) 
GROUP BY StudentId;
```

## Funktioner

### trim(string)

> Tar bort whitespace (mellanslag, radmatning, tab) från början och slut av en sträng. Exempelvis " , good, " blir ", good,"

### max(number)

> Räknar maximala värdet för ett set av värden.

### length(string)

> Räknar ut hur många tecken en sträng har

### replace(string, sökString, bytUtMedString)

> Ersätter string med 

### group_concat(fält)

> Slår ihop en grupps (GROUP BY) värden (rader) till en komma separerad sträng. 
