# PostgreSQL

### Table of Contents

- [Basics](#basics)
- [Console](#console)
- [Configuration](#configuration)
- [Data Types](#data-types)
- [Entity Relationships Diagram](#entity-relationships-diagram)
- [Tables](#tables)
- [Partitioning](#partitioning)
- [Roles](#roles)
- [References](#references)

### Basics
* PostgreSQL is a powerful, open-source object-relational database engine.
* For databases there are a few main elements:
    * Server: hardware that contains the db engine installed and running.
    * DB engine: software that provides a set of services to manage databases.
    * Database: a collection of tables, views, functions, etc.
    * Schemas: a collection of objects inside a database (tables, functions, relationships, sequences, etc.).
    * DB table: structure that organizes data in rows and columns.
* Based on object-relational model. Allows inheritance, references, etc.
* PostGIS is a geo-localization extension for PostgreSQL. Supports geographic objects and queries.
* PL/PgSQL allows to create functions and procedures in PostgreSQL.
* Supports ACID properties (Atomicity, Consistency, Isolation, Durability).
    * Atomicity: you can break an operation in small chunk that will execute one by one. If there is an error in one of these, a rollback mechanism will apply to revert incomplete changes.
    * Consistency: you can relate tables to make data consistent (foreign keys).
    * Isolation: operations are isolated from each other. You can execute reads and writes without interference.
    * Durability: data is saved even if the system crashes.

### Console

* Commands:
    * Help:
        * `\?`: help on console commands.
        * `\h command`: help on SQL commands.
    * Navigation:
        * `\l`: list databases.
        * `\c database_name`: connect to database.
        * `\dt`: list tables.
        * `\d table_name`: describe table.
        * `\dn`: list schemas.
        * `\df`: list functions.
        * `\dv`: list views.
        * `\du`: list roles.
    * Inspection and execution:
        * `\g`: execute last command.
        * `\s`: show history.
        * `\s file_name`: save history to file.
        * `\i file_name`: execute file.
        * `\e`: open editor.
        * `\ef function_name`: open function in editor.
    * Debug and optimization:
        * `\timing`: show time of queries.
    * Close console:
        * `\q`: quit

### Configuration

* postgresql.conf: main configuration file.
    * Location: `SHOW config_file;`.
    * Helpful for:
        * Configure replicas.
        * Limiting IP address to connect to the database.
        * Fix issues like:
            * No enough memory in server.
            * Process hasn't finished.
            * Roles number is exceeded.
            * CPU is 100% used.
* pg_hba.conf: authentication configuration file.
    * Location: `SHOW hba_file;`.
    * Helpful for:
        * Allows more specificity for the roles and access to databases for each user.
        * Contains a table with type (local, host, hostssl, hostnossl), database name, username, IP address and method (trust, md5, etc).
* pg_ident.conf: user mapping configuration file.
    * Location: `SHOW ident_file;`.
    * Useful for mapping OS user with PostgreSQL user.

### Data Types

Numeric:

* `smallint`: 2 bytes, -32768 to 32767.
* `integer`: 4 bytes, -2147483648 to 2147483647.
* `bigint`: 8 bytes, -9223372036854775808 to 9223372036854775807.
* `decimal`: variable, up to 131072 digits before the decimal point and up to 16383 digits after the decimal point.
* `numeric`: variable, up to 131072 digits before the decimal point and up to 16383 digits after the decimal point.
* `real`: 4 bytes, 6 decimal digits precision.
* `double precision`: 8 bytes, 15 decimal digits precision.

Currency:

* `money`: 8 bytes, currency amount.

Text:

* `character(n)`: fixed-length, blank-padded.
* `character varying(n)`: variable-length.
* `text`: variable-length.

Binary:

* `bytea`: variable-length binary string.

Date/Time:

* `timestamp`: date and time.
* `date`: date only.
* `time`: time only.
* `interval`: time interval.

Boolean:
* `boolean`: true or false.

Geometry:

* `point`: geometric point.
* `line`: geometric line.
* `lseg`: geometric line segment.
* `box`: geometric box.
* `path`: geometric path.
* `polygon`: geometric polygon.
* `circle`: geometric circle.

IP Address:

* `cidr`: IPv4 or IPv6 network address.
* `inet`: IPv4 or IPv6 host address.
* `macaddr`: MAC address.

Bit String:

* `bit(n)`: fixed-length bit string.
* `bit varying(n)`: variable-length bit string.

XML and JSON:

* `xml`: XML data.
* `json`: JSON data.
* `jsonb`: JSON data with binary format.

Arrays:

* `integer[]`: array of integers.
* `text[]`: array of texts.
* `point[]`: array of points.

### Entity Relationships Diagram

Describes a way to relate tables in a database by using lines to connect tables.

### Tables

Available actions:

* Create table:
    * `CREATE TABLE table_name (column_name data_type, column_name data_type);`
* Alter table:
    * `ALTER TABLE table_name ADD COLUMN column_name data_type;`
    * `ALTER TABLE table_name DROP COLUMN column_name;`
    * `ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name;`
    * `ALTER TABLE table_name ALTER COLUMN column_name SET NOT NULL;`
    * `ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL;`
    * `ALTER TABLE table_name ADD CONSTRAINT constraint_name UNIQUE (column_name);`
    * `ALTER TABLE table_name DROP CONSTRAINT constraint_name;`
* Drop table:
    * `DROP TABLE table_name;`

### Partitioning

* Helpful when tables are too large.
* Divide a large table into smaller, more manageable pieces.
* Internal partitioning: handled by the database engine.
* Will keep the same table name and logical structure.
* To create a partitioned table: `CREATE TABLE table_name (column_name data_type) PARTITION BY RANGE (column_name);`
* To create a partition: `CREATE TABLE table_name PARTITION OF parent_table_name FOR VALUES FROM (value) TO (value);`
* These table doesn't allow to create indexes, constraints (e.g. primary keys), etc. The use of these partitions are related to historic data so it's not necessary to have these constraints.

### Roles

* They can CREATE, ALTER and DROP other roles.
* Assign privileges to other roles.
* Group other roles.
* There is one default role: `postgres`.
* To create a user: `CREATE USER user_name WITH PASSWORD 'password';`
* To create a group role: `CREATE ROLE group_name NOLOGIN;`
* To add a user to a group: `GRANT group_name TO user_name;`
* To grant privileges to a group on a database:
    * `GRANT CONNECT ON DATABASE database_name TO group_name;`
    * `GRANT USAGE ON SCHEMA schema_name TO group_name;`
    * `GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA schema_name TO group_name;`
    * `GRANT USAGE ON ALL SEQUENCES IN SCHEMA schema_name TO group_name;`
* To set default privileges for future objects:
    * `ALTER DEFAULT PRIVILEGES IN SCHEMA schema_name GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO group_name;`
    * `ALTER DEFAULT PRIVILEGES IN SCHEMA schema_name GRANT USAGE ON SEQUENCES TO group_name;`

### Foreign Keys

Components:

* Origin table: table that contains the foreign key.
* Destination table: table that contains the primary key.
* Actions:
    * `CASCADE`: delete or update the row in the destination table.
    * `SET NULL`: set the foreign key to null.
    * `RESTRICT`: restrict the delete or update operation.
    * `NO ACTION`: same as `RESTRICT`.
    * `SET DEFAULT`: set the foreign key to the default value.

### References

* [PostgreSQL Data Types](https://www.postgresql.org/docs/11/datatype.html)
* [Most common data types](https://www.todopostgresql.com/postgresql-data-types-los-tipos-de-datos-mas-utilizados/)
* [Mockaroo](https://www.mockaroo.com/): generate random data.
