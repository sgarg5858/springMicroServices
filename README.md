# Spring-MicroServices #Spring-Cloud
Cloud Native

When you are deploying the entire application as a single WAR or JAR file,

It means you have been writing MONOLITHIC application.

Disadvantges of Monolithic applications:

  1. Even for a small change,the entire application has to be redeployed thus increasing the down time of an application.
  
  2. Larger the app,larger the deployment time and start up time.
  
  3. Even if only specific parts of an application are experiencing more traffic, we have to deploy the entire app on mutiple servers
  
     to achieve scaling.
     
  4. We are stuck with the chosen Technology,Later on we find the particular functionality can be implemented better with other tech.
  
  
What is MicroService?

  1.Microservice is an architectural style. 
  
  2.In this style, the application is made up of smaller ( or micro ) apps that communicate with each other, through like HTTP.
  
  3.Microservices are by nature distributed in nature. 
  
  
 Deploying an Application On cloud doesn't mean that the application is cloud-native.
 
 1. Will your application take advantges of multiple instances of related services and load balance accross them.
 
 2.In the cloud, a lot of things can go wrong. If a service is suddenly unavailable, is your application resilient enough?
 
 3. instances can be increased, decreased, moved around and so on. Will your application automatically adapt to such changes?
 
 If yes, then your application is cloud native.
 ********************************************************************************************************************************
 How to Design Database for microservices?
 
 1. Having a single Shared DB is Dangerous.If the DB fails entire application fails.Best Practice is to not to use a shared DB.
 
 2.Each MicroService should have it's own set of tables in separate database.
 
 3. One MicroService should not try to directly access tables owned by another microservice.If there is data dependency then 
 
 microservice should call another srvice through rest api.
 
 4. Remove Foreign Key Constraints and place tables in separate schemas.
 
 5.Remove JPA Entity Relationships between entities.
 
 6. Use Rest API calls to fetch data in case of data dependency.
 

 ********************************************************************************************************************************
 CLOUD CONFIG
 
 As we know In application.properties file we mention properties for database connection and some other properties which will be common 
 
 to all projects which we will make.
 
 1. Placing these properties in all files will lead to duplication and if we have to do any change then we will we have to make changes
 
    in every project.
    
  Solution:
  
  We will make git repository where we will create " application.properties " file and we can place all these properties there
  
  which will be accessed by all projects and if we make any changes in this file the change will be visible to all.
  
  
  Check application.properties file
  
  1. Create new SpringBoot Project Named ConfigServer  with dependency -> Config Server
  
 Then add properties>
 
1. server.port=1111

2.spring.application.name=ConfigServer

3.spring.cloud.config.server.git.uri=https://github.com/sgarg5858/spring-MicroServices.git
# If GitHub is using
#spring.cloud.config.server.git.uri=https://github.com/PlaygroundConfiguration/SpringMicroservices.git

UserName/Password:

spring.cloud.config.server.git.username=your_github_username
spring.cloud.config.server.git.password=your_github_password

******************************************************************************************************************************
Add @EnableConfigServer in the application file of ConfigServer
******************************************************************************************************************************

Imp: Role Of Config Server:

All the microservices connect to Config Server and recieves configuration file and Config Server Connects to the Github 
 
******************************************************************************************************************************
Create a bootstrap.properties file in each microservice.

spring.cloud.config.uri=http://localhost:1111
spring.datasource.username=root
spring.datasource.password=root
*******************************************************************************************************************************
 
