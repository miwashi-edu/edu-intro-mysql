# edu-intro-mysql

> Låna en MySQl Databas, detta görs bara en gång

```bash
docker pull mysql/mysql-server:latest
```

```bash
docker run --name iths-mysql\
           -e MYSQL_ROOT_PASSWORD=root\
           -e MYSQL_USER=iths\
           -e MYSQL_PASSWORD=iths\
           -e MYSQL_DATABASE=iths\
           -p 3306:3306\
           --tmpfs /var/lib/mysql\
           -d mysql/mysql-server:latest
```
## Docker prosesser

> Se vad som körs

```bash
docker ps
```

## Se vad som inte körs

> För att se containrar som stannat lägger vi till växeln -a

```bash
docker ps -a
```

## Starta MySql

```bash
docker start iths-mysql
```

## Stoppa mysql

```
docker stop iths-mysql
```

## Starta bash i vår container

```bash
docker exec -it iths-mysql bash
```
