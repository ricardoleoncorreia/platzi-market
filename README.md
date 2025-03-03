# Technologies used

* Spring Data JPA: connect to sql database
* Spring Security: authentication and authorization

# About Java

* OpenJDK is a free and open-source implementation of the Java Platform, Standard Edition.
* There are other alternatives like Amazon Corretto, RedHat OpenJDK, Oracle JDK, AdoptOpenJDK, etc.

# Tools Installation Links

* [Java OpenJDK](https://adoptium.net/)
* [IntelliJ IDEA](https://www.jetbrains.com/idea/download/?section=mac)
* [Insomnia](https://insomnia.rest/)
* [PostgreSQL](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads/)

# Autocontained Applications

* Spring Boo uses Tomcat (default), Jetty or Undertow servers.
* Leave all the configurations to Spring Boot.

# Project setup

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

# Open Project

* Import the project in IntelliJ IDEA
* Import `build.gradle`. Open as project.

# References

* [Common Application Properties](https://docs.spring.io/spring-boot/appendix/application-properties/index.html)
