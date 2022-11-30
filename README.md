# edu-intro-mysql

## Kör ett script i containern

```bash
docker ps -a # Se om containern är igång
docker start iths-mysql # Starta om den inte är igång
cd ws
cd sql # Gå till en katalog där du har scriptet

curl -L  https://gist.github.com/miwashiab/e39a3228f0b389b6f3eca1b8c613bb2e/raw/db.sql -o db.sql

docker exec -i iths-mysql mysql -uroot -proot < db.sql # Kör Scriptet
docker exec -it iths-mysql bash # Lägg till winpty om den klagar på tty
```

### I container

```bash
mysql -uroot -proot
```

### I MySql

```sql
show databases;
use Chinook;
show tables;
```

### [db.sql gist](https://gist.github.com/miwashiab/e39a3228f0b389b6f3eca1b8c613bb2e/raw/db.sql)
