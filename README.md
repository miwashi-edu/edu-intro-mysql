# edu-intro-mysql

## Hämta komma separerad datafil

```bash
cd ~
cd sql
curl -L  https://gist.github.com/miwashiab/d891a64c7f73f4c8c3b5cfee2b3de776/raw/denormalized-data.csv -o denormalized-data.csv
docker ps
docker start iths-mysql # Om den inte är igång
docker cp denormalized-data.csv iths-mysql:/var/lib/mysql-files
docker exec -it iths-mysql bash # Glöm inte winpty ifall tty problem
```

## I containern
```bash
cd /var/lib/mysql-files
ls # Kolla att vi lyckats kopiera vår fil
cat denormalized-data.csv
mysql -uroot -proot
```

## I mysql
```sql
grant
exit
```

## I containern
```bash
mysql -uiths -piths
```

## I mysql
```sql
USE iths;

CREATE TABLE `UNF` (
    `Id` DECIMAL(38, 0) NOT NULL,
    `Name` VARCHAR(26) NOT NULL,
    `Grade` VARCHAR(11) NOT NULL,
    `Hobbies` VARCHAR(25),
    `City` VARCHAR(10) NOT NULL,
    `School` VARCHAR(20) NOT NULL,
    `HomePhone` VARCHAR(15),
    `JobPhone` VARCHAR(15),
    `MobilePhone1` VARCHAR(15),
    `MobilePhone2` VARCHAR(15)
)  ENGINE=INNODB;

DESC UNF;

LOAD DATA INFILE '/var/lib/mysql-files/denormalized-data.csv' INTO TABLE UNF ;
    
LOAD DATA INFILE '/var/lib/mysql-files/denormalized-data.csv'
INTO TABLE UNF 
FIELDS TERMINATED BY ';'
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

SELECT * FROM UNF;

DELETE FROM UNF;

LOAD DATA INFILE '/var/lib/mysql-files/denormalized-data.csv'
INTO TABLE UNF 
CHARACTER SET ?
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

```

[MySql Character Sets](https://dev.mysql.com/doc/refman/8.0/en/charset-mysql.html)

[denormalized-data gist](https://gist.github.com/miwashiab/d891a64c7f73f4c8c3b5cfee2b3de776)
