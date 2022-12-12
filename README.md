# edu-intro-mysql

## Instruktioner

### I bash

```bash
cd ~
cd ws
cd sql
docker start iths-mysql
docker exec -i iths-mysql mysql -uroot -proot < db.sql
docker cp denormalized-data.csv iths-mysql:/var/lib/mysql-files
docker exec -it iths-mysql bash
```

### I container

```bash
mysql -uroot -proot
```

### I MySQL

```sql
GRANT ALL ON Chinook.* to 'iths'@'%';
GRANT FILE ON *.* TO 'iths'@'%';
SHOW GRANTS FOR iths;
```

### I bash

```bash
cd ~
cd ws
git clone https://github.com/[GitHub Användare]/db2022.git
cd db2022
touch normalisering.sql
vi normalisering.sql
git add normalisering.sql
git commit -m "Added first version of normalization"
git push
docker exec -i iths-mysql mysql -uiths -piths < normalizering.sql
```

## normalisering.sql
```sql
USE iths;

DROP TABLE IF EXISTS UNF;

CREATE TABLE `UNF` (
    `Id` DECIMAL(38, 0) NOT NULL,
    `Name` VARCHAR(26) NOT NULL,
    `Grade` VARCHAR(11) NOT NULL,
    `Hobbies` VARCHAR(25),
    `City` VARCHAR(10) NOT NULL,
    `School` VARCHAR(30) NOT NULL,
    `HomePhone` VARCHAR(15),
    `JobPhone` VARCHAR(15),
    `MobilePhone1` VARCHAR(15),
    `MobilePhone2` VARCHAR(15)
)  ENGINE=INNODB;

LOAD DATA INFILE '/var/lib/mysql-files/denormalized-data.csv'
INTO TABLE UNF
CHARACTER SET latin1
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

DROP TABLE IF EXISTS Student;

CREATE TABLE Student (
    Id INT NOT NULL,
    FirstName VARCHAR(255) NOT NULL,
    LastName VARCHAR(255) NOT NULL,
    CONSTRAINT PRIMARY KEY (Id)
)  ENGINE=INNODB;

INSERT INTO Student (ID, FirstName, LastName) SELECT DISTINCT Id, SUBSTRING_INDEX(Name, ' ', 1), SUBSTRING_INDEX(Name, ' ', -1) FROM UNF;

```
