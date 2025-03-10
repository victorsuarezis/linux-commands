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
$ pg_restore -v -h localhost -p 5432 -U postgres -d NOMBRE_BD "RUTA.backup"

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

## Agregar ***LIMIT*** a consultas dentro de codigo **iDempiere**
### Declaracion
```java
private static final String NATIVE_MARKER = "NATIVE_"+Database.DB_POSTGRESQL+"_KEYWORK";
```
o
```java
private static final String NATIVE_MARKER = "NATIVE_PostgreSQL_KEYWORK";
```

### Implementacion
```java
String sql = "SELECT COLUMN FROM TABLE WHERE COLUMNA =? ORDER BY 1 DESC "+NATIVE_MARKER+"LIMIT 1"+NATIVE_MARKER;
```


Rol de lectura y escritura
El proceso de agregar un rol de lectura/escritura es muy similar al proceso de rol de solo lectura que se trató anteriormente. El primer paso es crear un rol:

```sql
CREATE ROLE readwrite LOGIN;
```

Conceda permiso a este rol para conectarse a la base de datos de destino:
```sql
GRANT CONNECT ON DATABASE mydatabase TO readwrite;
```

Conceda privilegio de uso de esquemas:
```sql
GRANT USAGE ON SCHEMA myschema TO readwrite;
```

Si desea permitir que este rol cree nuevos objetos como tablas de este esquema, utilice el siguiente SQL en lugar del anterior:
```sql
GRANT USAGE, CREATE ON SCHEMA myschema TO readwrite;
```

El siguiente paso es conceder acceso a las tablas. Como se mencionó en la sección anterior, la concesión puede realizarse en tablas individuales o en todas las tablas del esquema. Para tablas individuales, utilice el siguiente SQL:
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLE mytable1, mytable2 TO readwrite;
```

Para todas las tablas y vistas del esquema, utilice el siguiente SQL:
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA myschema TO readwrite;
```

Para conceder automáticamente permisos sobre tablas y vistas añadidas en el futuro:
```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA myschema GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO readwrite;
```

Para los roles de lectura y escritura, normalmente existe el requisito de utilizar secuencias también. Puede dar acceso selectivo de la siguiente manera:
```sql
GRANT USAGE ON SEQUENCE myseq1, myseq2 TO readwrite;
```

También puede conceder permiso a todas las secuencias mediante la siguiente instrucción SQL:
```sql
GRANT USAGE ON ALL SEQUENCES IN SCHEMA myschema TO readwrite;
```

Para conceder permisos automáticamente a las secuencias añadidas en el futuro:
```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA myschema GRANT USAGE ON SEQUENCES TO readwrite;
```

Puede conceder más o menos permisos en función de los requisitos. La documentación del comando GRANT de PostgreSQL proporciona más detalles sobre los objetos en los que se pueden conceder permisos y las sentencias SQL necesarias.

Creación de usuarios de base de datos
Con los roles implementados, se simplifica el proceso de creación de usuarios. Simplemente crea el usuario y concédele uno de los roles existentes. Estas son las instrucciones SQL para este proceso:
```sql
CREATE USER myuser1 WITH PASSWORD 'secret_passwd' LOGIN;
GRANT readonly TO myuser1;
```

### CERRAR SESIONES ACTIVAS
```sql
SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity
WHERE datname = 'myDataBase' AND pid <> pg_backend_pid();
```

### SQL para monitorear peso de Base de Datos
```sql
postgres=# SELECT pg_database.datname, pg_size_pretty(pg_database_size(pg_database.datname)) AS SIZE FROM pg_database;
```

### SQL para monitorear peso de Tablas en una Base de Datos
```sql
SELECT relname as "Table", pg_size_pretty(pg_total_relation_size(relid)) As "Size", pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) as "External Size" FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC;
```
