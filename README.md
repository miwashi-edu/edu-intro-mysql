# edu-intro-mysql

## Beskrivning

> Importera flatfil (komma-separerad fil), in i mysql. Träning i främst DDL, DCL samt Teckenuppsättningar. Vi nämner även InnoDB och att mysql kan använda andra databas-motorer än den.

## Hämta komma-separerad datafil

```bash
cd ~
cd sql //Istället för mappen sql, kan du clona ditt inlämningsrepo från github och använda den istället.
curl -L  https://gist.github.com/miwashiab/d891a64c7f73f4c8c3b5cfee2b3de776/raw/denormalized-data.csv -o denormalized-data.csv
docker ps
docker start container-with-mysql # Om den inte är igång
docker cp denormalized-data.csv container-with-mysql:/var/lib/mysql-files
docker exec -it container-with-mysql bash # Glöm inte winpty ifall tty problem
```
> ### Note ### 
## I container-with-mysql
```bash
cd /var/lib/mysql-files
ls # Kolla att vi lyckats kopiera vår fil
cat denormalized-data.csv
mysql -uroot -proot
```

## I mysql
```sql
SHOW GRANTS FOR iths;
GRANT ALL PRIVILEGES ON iths.* TO 'iths'@'%';
GRANT ALL PRIVILEGES ON Chinook.* TO 'iths'@'%';

/* Rättigheter att ladda data fil */
GRANT FILE ON *.* TO 'iths'@'%';
SHOW GRANTS FOR iths;
exit
```
 > **_NOTE:_** __%__ ör wildcard, och då den står efter @, betyder det att alla nätverk är tillåtna att ansluta.
 
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
    `School` VARCHAR(30) NOT NULL,
    `HomePhone` VARCHAR(15),
    `JobPhone` VARCHAR(15),
    `MobilePhone1` VARCHAR(15),
    `MobilePhone2` VARCHAR(15)
)  ENGINE=INNODB;

DESC UNF;

LOAD DATA INFILE '/var/lib/mysql-files/denormalized-data.csv'
INTO TABLE UNF 
CHARACTER SET latin1
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

SELECT * FROM UNF;

DELETE FROM UNF;
```

 > **_NOTE:_** Notera ENGINE=[INNODB](https://en.wikipedia.org/wiki/InnoDB), Mysql kan använda olika databasmotorer. InnoDB är den som är default sedan Mysql 5. 

[MySql Character Sets](https://dev.mysql.com/doc/refman/8.0/en/charset-mysql.html)  

[denormalized-data gist](https://gist.github.com/miwashiab/d891a64c7f73f4c8c3b5cfee2b3de776)  
