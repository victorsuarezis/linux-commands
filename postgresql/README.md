# Comandos para PostgreSQL

#### Listar base de datos:
```shell
$ psql -l
```

#### Mostrar ayuda para backup:
```shell
$ pg_dump -?
$ pg_restore -?
$ psql -?
```

#### Backup tipo custom:
```shell
$ pg_dump -i -h localhost -p 5432 -U postgres -F c -b -v -f "RUTA.backup" NOMBRE_BD

-p, --port=PORT database server port number
-i, --ignore-version proceed even when server version mismatches
-h, --host=HOSTNAME database server host or socket directory
-U, --username=NAME connect as specified database user
-W, --password force password prompt (should happen automatically)
-d, --dbname=NAME connect to database name
-v, --verbose verbose mode
-F, --format=c|t|p output file format (custom, tar, plain text)
-c, --clean clean (drop) schema prior to create
-b, --blobs include large objects in dump
-v, --verbose verbose mode
-f, --file=FILENAME output file name
```

Restore tipo custom:
```shell
$ pg_restore -i -v -h localhost -p 5432 -U postgres -d NOMBRE_BD "RUTA.backup"

-p, --port=PORT database server port number
-i, --ignore-version proceed even when server version mismatches
-h, --host=HOSTNAME database server host or socket directory
-U, --username=NAME connect as specified database user
-W, --password force password prompt (should happen automatically)
-d, --dbname=NAME connect to database name
-v, --verbose verbose mode
```


####  Restore tipo custom y crear DB:
(creará la bd que se referencia dentro del backup y usa la bd postgres para conectarse):
```shell
$ pg_restore -i -h localhost -p 5432 -U postgres -C -d postgres -v "RUTA.backup"

-p, --port=PORT database server port number
-i, --ignore-version proceed even when server version mismatches
-h, --host=HOSTNAME database server host or socket directory
-U, --username=NAME connect as specified database user
-W, --password force password prompt (should happen automatically)
-d, --dbname=NAME connect to database name
-v, --verbose verbose mode
-C, --create Create database
```

#### Backup tipo plain:
```shell
$ pg_dump -i -h localhost -p 5432 -U postgres -F p -b -v -f "RUTA.sql" NOMBRE_BD

-p, --port=PORT database server port number
-i, --ignore-version proceed even when server version mismatches
-h, --host=HOSTNAME database server host or socket directory
-U, --username=NAME connect as specified database user
-W, --password force password prompt (should happen automatically)
-d, --dbname=NAME connect to database name
-v, --verbose verbose mode
-F, --format=c|t|p output file format (custom, tar, plain text)
-c, --clean clean (drop) schema prior to create
-b, --blobs include large objects in dump
-v, --verbose verbose mode
-f, --file=FILENAME output file name
```

#### Restore tipo plain:
```shell
$ psql -h localhost -p 5432 -U postgres -d NOMBRE_BD -f "RUTA.backup"

-p, --port=PORT database server port number
-h, --host=HOSTNAME database server host or socket directory
-U, --username=NAME connect as specified database user
-W, --password force password prompt (should happen automatically)
-d, --dbname=NAME connect to database name
-f, --file file
```

#### Restore Psql dmp
```shell
psql -h localhost -p 5432 -d NOMBRE_BD -U USUARIO -f archivo.dmp
```

### Backup DMP
```shell
pg_dump -h localhost -p 5432 --no-owner -U USUARIO NAME_DATABASE > /opt/ExpDat.dmp 
```
Luego ubicarse en la carpeta del respaldo: cd /opt/ y comprimir con el comando: 
```shell
jar cvfM ExpDat.jar ExpDat.dmp
```

### Descomprimir Jar

```shell
jar xfv ExpDat.jar
```

### ALTER ROLE

El manejo de roles en PostgreSQL permite diferentes configuraciones, entre ellas estan:

- SUPERUSER/NOSUPERUSER. Super usuario, privilegios para crear bases de datos y usuarios.
- CREATEDB/NOCREATEDB. Permite crear bases de datos.
- CREATEROLE/NOCREATEROLE. Permite crear roles.
- CREATEUSER/NOCREATEUSER. Permite crear usuarios.
- LOGIN/NOLOGIN. Este atributo hace la diferencia entre un rol y usuario. Ya que el usuario tiene permisos para acceder a la base de datos a traves de un cliente.
- PASSWORD. Permite alterar la contraseña.
- VALID UNTIL. Expiración de usuarios.
**Ejemplo:** `ALTER ROLE nombre_usuario WITH LOGIN`