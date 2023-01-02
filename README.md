# edu-intro-mysql

## Beskrivning

> Vi kör ett SQL-script i vår realations databas (Mysql) som finns i en docker container.
> Databasen är Sql-lites övningsdatabas och innehåller data för en affär Chinook, som säljer musik.
> I slutet hittar du frågor där du ska använda DQL för att besvara dem.

## Instruktioner

### I bash (gitbash)

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


### Frågor

frågor
Hur många album finns det?  
Hur många artister finns det?  
Hur många låtar finns det?  
Hur många anställda har skivaffären Chinook?  
Hur många kunder har skivaffären?  
Vilka mediatyper säljer skivaffären?  
---
Hur många försäljningar har de gjort?  
Hur  många låtar har de sålt?  
När  gjordes första försäljningen?  
När  gjordes senaste försäljningen?  
Hur många försäljningar gjorde de 2013?  

Hur många städer har de kunder i?  
Hur många kunder har de i Stockholm?  
Hur många länder har de kunder i?  
Hur många kunder har de i Sverige?  
Hur  många kunder har de i England?  

--- (frågor med join)
Hur många Album har AC/DC? 
tips: from Artist join Album on Artist.ArtistId = Album.ArtistId;  
alt: from Artist join Album using (ArtistId);  

--- (frågor med aggregering)
Vilken artist har mest låtar?  
Vilken kund har gjort största inköp?  
Vad handlar kunder i genomsnitt för?  
Hur många låtar finns i genomsnitt per album?  
Hur många låtar har varje artist i genomsnitt?  
Vad är maximalt antal låtar en artist har?  
