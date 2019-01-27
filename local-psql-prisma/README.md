## Create Databe in/and docker container - see [article](https://medium.com/@lvthillo/connect-from-local-machine-to-postgresql-docker-container-f785f00461a7)

```
    -> create container
$ docker run -d -p 5432:5432 --name docker-container-name -e POSTGRES_PASSWORD=mysecretpassword postgres

    -> get "inside" the container
$ docker exec -it docker-container-name bash

    -> access postgres and create database
$ root@cb9222b1f718:/# psql -U postgres
psql (10.3 (Debian 10.3-1.pgdg90+1))
Type "help" for help.

$ postgres=# CREATE DATABASE mytestdb;
CREATE DATABASE

    -> exit container but it will continue to run
$ postgres=# \q
```

<strong>Now you have to install some PostgreSQL Client tool, like: </strong>

- PgAdmin (I'm using this one) - [download](https://www.pgadmin.org/download/) the `.dmg` extesion
- PSQL (PSQL)
- ...

<strong>Ps.:</strong>

- serverIP = where your container is runing (if you've installed docker tools, you can see all these configurations in kitmatic (normaly it is `localhost` or , my case, `192.168.99.100`)).

```
    -> connect container with data by user
$ psql -h serverIP -p 5432 -U postgres -W
Password for user postgres:
Type "help" for help.

$ postgres=# \l
                    List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |
-----------+----------+----------+------------+------------+
 mytestdb  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
           |          |          |            |            |
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
           |          |          |            |            |
(4 rows)
```

- Open PgAdmin
- Click in server with rigth button of the mouse
- Pass the mouse over the `Create` option and click in `Server`
- Set some name, go to `connection` session and fill out
  - `host:` serverIP (192.168.99.100)
  - `port:` port (5432)
  - `maintenance database:` database name (mytestdb)
  - `username:` database user (postgres)
  - `password:` secret passwor (mysecretpassword)
- Save the connection configuration to connect to the database running in your docker container

## Add prisma layer locally - see (documentation)[https://www.prisma.io/]

<strong>PS.:</strong>

- I'm goint to fill out the config above with my answers, please ensure to replace with yours

```
$ prisma init

? Set up a new Prisma server or deploy to an existing server
    - Use existing database
? What kind of database do you want to deploy to?
    - PostgreSQL

? Does your database contain existing data? No
? Enter database host 192.168.99.100
? Enter database port 5432
? Enter database user postgres
? Enter database password mysecretpassword
? Enter database name mytestdb
? Use SSL? No

? Select the programming language for the generated Prisma c
lient Don't generate
```

<strong>PS.:</strong>

- Double check the `endpoint` in `prisma.yml` file and fix if necessary (serverIP:port)

```
    -> Create and start Prisma server
$ docker-compose up -d

    -> Deploy prisma server locally
$ prisma deploy

```

- Now you can go to you local environment and get prisma layer know better.
