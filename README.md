# edu-intro-mysql

> Normalisera Hobbies

## Funktioner

### trim(string)

> Tar bort whitespace (mellanslag, radmatning, tab) från början och slut av en sträng. Exempelvis " , good, " blir ", good,"

### max(number)

> Räknar maximala värdet för ett set av värden.

### length(string)

> Räknar ut hur många tecken en sträng har

### replace(string, sökString, bytUtMedString)

> Ersätter string med 

### group_concat(string, sökString, bytUtMedString)

> Slår ihop en grupps (GROUP BY) värden (rader) till en komma separerad sträng. 

## Analys

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

```sql
SELECT Id, "We want first from all!", HOBBIES FROM UNF
WHERE Trim(SUBSTRING_INDEX(Hobbies, ",", 1))
;
```

## Alla mittersta fält

```sql
SELECT Id, "We want middle when three!", Hobbies FROM UNF
WHERE (LENGTH(Hobbies) - LENGTH(REPLACE(Hobbies, ',', ''))=2)
;
```

## Alla sista fält

```sql
SELECT Id, "We want last when more than one!", Hobbies FROM UNF
WHERE (LENGTH(Hobbies) - LENGTH(REPLACE(Hobbies, ',', ''))>=1)
```

## Alternativa WHERE satser

> Exprerimentera med dessa.

```sql
where Hobbies not like "%,%,%" and Hobbies like "%,%";
where Hobbies not like "%,%";
where Hobbies like '%,';
```

## Do it

```sql
First: trim(substring_index(Hobbies, ",", 1))
Middle: trim(substring_index(substring_index(Hobbies, ",", -2),"," ,1))
Last: trim(substring_index(Hobbies, ",", -1))

SELECT Id as StudentId, trim(SUBSTRING_INDEX(Hobbies, ",", 1)) AS Hobby FROM UNF;
SELECT Id as StudentId, trim(substring_index(substring_index(Hobbies, ",", -2),"," ,1)) AS Hobby FROM UNF;
SELECT Id as StudentId, trim(substring_index(Hobbies, ",", -1)) AS Hobby FROM UNF;
```
