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
        * Contains a table with type (local, host, hostssl, hostnossl), database name, username, IP address and method (trust, md5, etc.).
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

### Joins

* Inner join: `SELECT * FROM table1 INNER JOIN table2 ON table1.column = table2.column;`
* Left join: `SELECT * FROM table1 LEFT JOIN table2 ON table1.column = table2.column;`
* Right join: `SELECT * FROM table1 RIGHT JOIN table2 ON table1.column = table2.column;`
* Left outer join: `SELECT * FROM table1 LEFT OUTER JOIN table2 ON table1.column = table2.column;`
* Right outer join: `SELECT * FROM table1 RIGHT OUTER JOIN table2 ON table1.column = table2.column;`
* Full outer join: `SELECT * FROM table1 FULL OUTER JOIN table2 ON table1.column = table2.column;`

### Special Functions

* __ON CONFLICT DO__. Tells what to do when a conflict occurs.
    * `INSERT INTO table_name (column1, column2) VALUES (value1, value2) ON CONFLICT (column1) DO NOTHING;`
    * `INSERT INTO table_name (column1, column2) VALUES (value1, value2) ON CONFLICT (column1) DO UPDATE SET column2 = value2;`
* __RETURNING__. Returns the inserted or updated row.
    * `INSERT INTO table_name (column1, column2) VALUES (value1, value2) RETURNING *;`
    * `UPDATE table_name SET column1 = value1 WHERE column2 = value2 RETURNING *;`
* __LIKE__. Search for a pattern.
    * `SELECT * FROM table_name WHERE column_name LIKE 'pattern';`
    * `SELECT * FROM table_name WHERE column_name LIKE 'pattern%' ESCAPE 'escape_character';`
* __ILIKE__. Case-insensitive search for a pattern.
    * `SELECT * FROM table_name WHERE column_name ILIKE 'pattern';`
    * `SELECT * FROM table_name WHERE column_name ILIKE 'pattern%' ESCAPE 'escape_character';`
* __IS__ && __IS NOT__. Check if a value is null.
    * `SELECT * FROM table_name WHERE column_name IS NULL;`
    * `SELECT * FROM table_name WHERE column_name IS NOT NULL;`

### Advanced Functions

* __COALESCE__. Returns the first non-null value.
    * `SELECT COALESCE(column_name, 'default_value') FROM table_name;`
* __NULLIF__. Returns null if two values are equal.
    * `SELECT NULLIF(column1, column2) FROM table_name;`
* __GREATEST__. Returns the greatest value.
    * `SELECT GREATEST(column1, column2) FROM table_name;`
* __LEAST__. Returns the least value.
    * `SELECT LEAST(column1, column2) FROM table_name;`
* ANONYMOUS BLOCKS. Allows to create a block of code.
    * `CASE WHEN condition THEN statement ELSE statement END;`

### Views

* Materialized view: stores the result of the query in memory. Helpful for queries on sets of data that won't change (e.g. yesterday's results).
  * Create view: `CREATE MATERIALIZED VIEW view_name AS SELECT * FROM table_name;`
  * Use view: `SELECT * FROM view_name;`
  * Refresh view: `REFRESH MATERIALIZED VIEW view_name;`
* Volatile view: doesn't store the result of the query. Data is always fresh.
  * Create view: `CREATE VIEW view_name AS SELECT * FROM table_name;`
  * Use view: `SELECT * FROM view_name;`

### Stored Procedures (PL/pgSQL)

Allows to create functions and procedures. Some examples:

```sql
CREATE OR REPLACE FUNCTION function_name (parameter1 data_type, parameter2 data_type)
RETURNS data_type AS $$
DECLARE
    variable data_type;
BEGIN
    FOR variable IN SELECT * FROM table_name LOOP
        RAISE NOTICE 'Value: %', variable.column_name;
        variable := variable + 1;
    END LOOP;
    variable := variable + parameter1 + parameter2;
    RETURN variable;
END;
$$ LANGUAGE plpgsql;
```

```sql
SELECT function_name(1, 2);
```

```sql
DROP FUNCTION IF EXISTS function_name(data_type);
```

### Triggers

A trigger is a function that is executed when a specific event occurs (e.g. insert, update, delete).

```sql
CREATE OR REPLACE FUNCTION function_name ()
RETURNS TRIGGER AS $$
BEGIN
    IF TG_OP = 'INSERT' THEN
        RAISE NOTICE 'Inserting row';
    ELSIF TG_OP = 'UPDATE' THEN
        RAISE NOTICE 'Updating row';
    ELSIF TG_OP = 'DELETE' THEN
        RAISE NOTICE 'Deleting row';
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

```sql
CREATE TRIGGER trigger_name
AFTER INSERT OR UPDATE OR DELETE ON table_name
FOR EACH ROW
EXECUTE FUNCTION function_name();
```

```sql
DROP TRIGGER IF EXISTS trigger_name ON table_name;
```

### Connect to remote server

* An extension is needed to connect to a remote server. For example, `dblink` by using `CREATE EXTENSION dblink;`.
* Usage is a below:

```sql
SELECT * FROM dblink('host=remote_host
    port=remote_port
    user=remote_user
    password=remote_password
    dbname=remote_db',
    'SELECT * FROM remote_table'
) AS remote_data (column1 data_type, column2 data_type);
```

### Transactions

* A transaction is a set of operations that are executed as a single unit.
* It is useful to ensure that all operations are executed when there is no error. Otherwise, a rollback will be executed.
* In the logic, the transaction can end with a `COMMIT` or `ROLLBACK`.

```sql
BEGIN;
INSERT INTO table_name (column1, column2) VALUES (value1, value2);
UPDATE table_name SET column1 = value1 WHERE column2 = value2;
DELETE FROM table_name WHERE column1 = value1;
COMMIT;
```

### Extensions

* Extensions are additional features that can be added to PostgreSQL.
* They are already installed but not enabled. You can enable them by using `CREATE EXTENSION extension_name;`.
* List of extensions can be reviewed by using `SELECT * FROM pg_available_extensions;` or in the [documentation](https://www.postgresql.org/docs/17/contrib.html).

### Backup and Restore

* Backup:
    * `pg_dump -U user_name -h host_name -d database_name -f file_name.sql`
    * `pg_dumpall -U user_name -h host_name -f file_name.sql`
* Restore:
    * `psql -U user_name -h host_name -d database_name -f file_name.sql`
    * `psql -U user_name -h host_name -f file_name.sql`

### Maintenance

* Vacuum:
  * Reclaims storage occupied by dead tuples: `VACUUM table_name;`
  * Has 3 strategies:
    * `VACUUM (FULL) table_name;`: reclaims storage and reorders the table.
    * `VACUUM (FREEZE) table_name;`: freezes the table.
    * `VACUUM (ANALYZE) table_name;`: collects statistics.
* Analyze: collects statistics about the contents of tables.
    * `ANALYZE table_name;`
* Reindex: rebuilds indexes.
    * `REINDEX table_name;`
* Cluster: reorders the table based on an index.
    * `CLUSTER table_name USING index_name;`

### Replication

* Master-Slave:
    * Master: main server that receives write operations.
    * Slave: secondary server that receives read operations.
    * Configuration:
        * Set up master server:
            * `postgresql.conf`:
                * `wal_level = logical`: write-ahead log level.
                * `max_wal_senders = 3`: maximum number of senders.
                * `archive_mode = on`: archive mode.
                * `archive_command = 'cp %p /path/to/archive/%f'`: archive command.
            * `pg_hba.conf`:
                * `host replication user_name slave_ip/32 md5`
        * Set up slave server:
            * `postgresql.conf`:
                * `hot_standby = on`: allows read-only queries.
            * `pg_basebackup -h host_name -R -D /path/to/data -U user_name`: backup the master.
        * Other configurations:
            * `wal_keep_segments = 8`: number of segments to keep.
            * `max_replication_slots = 3`: maximum number of replication slots.
            * `max_wal_size = 1GB`: maximum size of write-ahead log.
            * `min_wal_size = 80MB`: minimum size of write-ahead log.
            * `synchronous_standby_names = 'slave1, slave2'`: synchronous replication.
        * Other commands:
            * `pg_ctl start -D /path/to/data`: start the server.
            * `pg_ctl stop -D /path/to/data`: stop the server.
            * `pg_ctl reload -D /path/to/data`: reload the server.
            * `pg_ctl status -D /path/to/data`: check the status of the server.

**Notes**:

* On replica databases, it won't be possible to execute write operations.
* Once the replica is set up, it will have the same configs as the master (e.g. password).

### References

* [PostgreSQL Data Types](https://www.postgresql.org/docs/11/datatype.html)
* [Most common data types](https://www.todopostgresql.com/postgresql-data-types-los-tipos-de-datos-mas-utilizados/)
* [Mockaroo](https://www.mockaroo.com/): generate random data.
