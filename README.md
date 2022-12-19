# edu-intro-mysql

> Normalisera Phones

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

## normalisering.sql
```sql
DROP TABLE IF EXISTS Phone;
CREATE TABLE Phone (
    PhoneId INT NOT NULL AUTO_INCREMENT,
    StudentId INT NOT NULL,
    Type VARCHAR(32),
    Number VARCHAR(32) NOT NULL,
    CONSTRAINT PRIMARY KEY(Id)
);

INSERT INTO Phone(StudentId, Type, Number) 
SELECT ID As StudentId, "Home" AS Type, HomePhone as Number FROM UNF
WHERE HomePhone IS NOT NULL AND HomePhone != ''
UNION SELECT ID As StudentId, "Job" AS Type, JobPhone as Number FROM UNF
WHERE JobPhone IS NOT NULL AND JobPhone != ''
UNION SELECT ID As StudentId, "Mobile" AS Type, MobilePhone1 as Number FROM UNF
WHERE MobilePhone1 IS NOT NULL AND MobilePhone1 != ''
UNION SELECT ID As StudentId, "Mobile" AS Type, MobilePhone2 as Number FROM UNF
WHERE MobilePhone2 IS NOT NULL AND MobilePhone2 != ''
;

CREATE VIEW PhoneList AS SELECT StudentId, group_concat(Number) FROM Phone GROUP BY StudentId;
```
