# Hello (Java/Spring Boot)

This is a `Java/Spring Boot` template for basic spring boot java8 application. With controller endpoint `'/hello'`
Repository structure

# The main files in this repository are:

*  `Controller.java` which is placed in src and which triggers contains end point Hello
*   Target contains `jar` file which we deploy in Docker container
*  `Pom.xml` resolves dependencies and plugins
# Usage

* To run application  pull repo to laptop
* Make sure below software are avilable 
  JDK8
  mvn
*  run the following command in cmd to start your application:
  `mvn spring-boot:run`
   Your application should start up locally on port 8080. If you visit http://localhost:8080 in your browser, Endpoint will return the String `Hello`
