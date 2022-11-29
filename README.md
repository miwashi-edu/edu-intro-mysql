# edu-intro-mysql

## Kör ett script i containern

```bash
docker ps -a # Se om containern är igång
docker start iths-mysql # Starta om den inte är igång
cd ws
cd sql # Gå till en katalog där du har scriptet
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
