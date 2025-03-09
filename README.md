# Spring Boot

### Technologies used

* Spring Data JPA: connect to sql database
* Spring Security: authentication and authorization

### About Java

* OpenJDK is a free and open-source implementation of the Java Platform, Standard Edition.
* There are other alternatives like Amazon Corretto, RedHat OpenJDK, Oracle JDK, AdoptOpenJDK, etc.

### Tools Installation Links

* [Java OpenJDK](https://adoptium.net/)
* [IntelliJ IDEA](https://www.jetbrains.com/idea/download/?section=mac)
* [Insomnia](https://insomnia.rest/)
* [PostgreSQL](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads/)

### Autocontained Applications

* Spring Boo uses Tomcat (default), Jetty or Undertow servers.
* Leave all the configurations to Spring Boot.

### Project setup

* Generate app in [Spring Initializr](https://start.spring.io/)
* Packages to include:
    * Gradle
    * Spring Web
* JDK version validation:
    * Right click on project name
    * Click on "Open Module Settings"
    * Go to "Project"
    * Check project SDK version
    * Go to Modules and select to exclude `.idea` and `gradle` folders.
    * In IDE file explorer, open settings (gear icon) and select if you want to see hidden files.

### Open Project

* Import the project in IntelliJ IDEA
* Import `build.gradle`. Open as project.

### JPA

* JPA is a specification for accessing, persisting, and managing data between Java objects and relational databases (ORMs).
* Some JPA implementations are Hibernate, EclipseLink, TopLink and ObjectDB.
* It uses annotations like `@Entity`, `@Table`, `@Column`, `@Id`, `@EmbededId`, `@GeneratedValue`, `@OneToMany`, `@ManyToOne`, etc.

### Spring Data

* Contains support for data technologies like SQL, MongoDB, Redis, Cassandra, etc.
* Spring Data JPA:
  * Provides repository support for the Java Persistence API (JPA).
  * Repositories are interfaces with methods supporting creating, reading, updating, and deleting records (JPARepository, CrudRepository & PagingAndSortingRepository).
  * Provide transparent auditing, query methods, and derived queries.

# PostgreSQL

### Basics
* PostgreSQL is a powerful, open-source object-relational database engine.
* For databases there are three important elements: language (SQL), engine (PostgreSQL) and server (hardware).
* Based on object-relational model. Allows inheritance, references, etc.
* PostGIS is a geo-localization extension for PostgreSQL. Supports geographic objects and queries.
* PL/PgSQL allows to create functions and procedures in PostgreSQL.
* Supports ACID properties (Atomicity, Consistency, Isolation, Durability).
  * Atomicity: you can break an operation in small chunk that will execute one by one. If there is an error in one of these, a rollback mechanism will apply to revert incomplete changes.
  * Consistency: you can relate tables to make data consistent.
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

### Configuration

* How to find configuration files:
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

* Numeric:
  * `smallint`: 2 bytes, -32768 to 32767.
  * `integer`: 4 bytes, -2147483648 to 2147483647.
  * `bigint`: 8 bytes, -9223372036854775808 to 9223372036854775807.
  * `decimal`: variable, up to 131072 digits before the decimal point and up to 16383 digits after the decimal point.
  * `numeric`: variable, up to 131072 digits before the decimal point and up to 16383 digits after the decimal point.
  * `real`: 4 bytes, 6 decimal digits precision.
  * `double precision`: 8 bytes, 15 decimal digits precision.
* Currency:
  * `money`: 8 bytes, currency amount.
* Text:
  * `character(n)`: fixed-length, blank-padded.
  * `character varying(n)`: variable-length.
  * `text`: variable-length.
* Binary:
  * `bytea`: variable-length binary string.
* Date/Time:
  * `timestamp`: date and time.
  * `date`: date only.
  * `time`: time only.
  * `interval`: time interval.
* Boolean:
  * `boolean`: true or false.
