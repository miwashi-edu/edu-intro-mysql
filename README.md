# edu-intro-mysql

## Beskrivning 

> Vi lånar en relationsdatabas. Detta gör vi via docker. Vi använder [run](https://docs.docker.com/engine/reference/commandline/run/) kommandot för att skapa en [image](https://docs.docker.com/engine/reference/commandline/images/) för en [container](https://www.docker.com/resources/what-container). Därefter använder vi [start](https://docs.docker.com/engine/reference/commandline/start/) och [stop](https://docs.docker.com/engine/reference/commandline/stop/) kommandon på vår container för att ha tillgång till den mysql databas som finns i den.

## Förberedelser

## Instruktioner

```bash
docker pull mysql/mysql-server:latest
```

```bash
docker run --name container-with-mysql\
           -e MYSQL_ROOT_PASSWORD=root\
           -e MYSQL_USER=auser\
           -e MYSQL_PASSWORD=iths\
           -e MYSQL_DATABASE=iths\
           -p 3306:3306\
           --tmpfs /var/lib/mysql\
           -d mysql/mysql-server:latest
```
-e = environment variable 
-p = port (din dators port: docker containerns port) För mysql är alltid porten 3306 på container sidan.
-d = detatched, dvs den startar i bakgrunden (daemon) och du kan fortsätta jobba i terminalen.
--name = ditt namn på containern, utelämnar du denna får du ett slumpmässigt namn.

> **_NOTE:_**  [--tmpfs](https://docs.docker.com/storage/tmpfs/) kan ställa till problem, ta bort den i sådana fall.

## Docker processer

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
docker start container-with-mysql
```

## Stoppa mysql

```
docker stop container-with-mysql
```

## Starta bash i vår container

```bash
docker exec -it iths-mysql bash
```
