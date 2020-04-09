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
  
1.   We will make git repository where we will create " application.properties " file and we can place all these properties there
  
  which will be accessed by all projects and if we make any changes in this file the change will be visible to all.
  
2.  For Every MicroService we will create  spring.application.name.properties file which will contain properties specific for every 
  
  microservice
  
  
  Check application.properties file
  
  1. Create new SpringBoot Project Named ConfigServer  with dependency -> Config Server
  
  with dependency spring-cloud-config-server
  
 Then add properties>
 
1. server.port=1111

2.spring.application.name=ConfigServer

3.spring.cloud.config.server.git.uri=https://github.com/sgarg5858/spring-MicroServices   if infygit .git

UserName/Password:

spring.cloud.config.server.git.username=your_github_username
spring.cloud.config.server.git.password=your_github_password

******************************************************************************************************************************
Add @EnableConfigServer Annotation  in the application file of ConfigServer
******************************************************************************************************************************

After Doing these two steps we can check if ConfigServer is Working Or Not?

http://localhost:1111/application/default    port no / application file/ default profile

It gives all the data listed in application.properties

{"name":"application","profiles":["default"],"label":null,"version":"4097bc00dd88deb369eb0fab40d35679be2c3bd8","state":null,"propertySources":[{"name":"https://github.com/sgarg5858/springMicroServices/application.properties","source":{"spring.datasource.url":"jdbc:oracle:thin:@127.0.0.1:1521:xe","spring.datasource.username":"system","spring.datasource.password":"1234","spring.jpa.hibernate.ddl-auto":"update","spring.datasource.driverClassName":"oracle.jdbc.driver.OracleDriver","spring.jpa.database-platform":"org.hibernate.dialect.Oracle10gDialect"}}]}

***************************************************************************************************************************
Imp: Role Of Config Server:

All the microservices connect to Config Server and recieves configuration file and Config Server Connects to the Github 
 
******************************************************************************************************************************
Updating MicroServices:

Now we have to Update Our MicroServices so that they can connect to Config Sever recieve the properties from Git.

When we use Cloud-Config, each microservice gets two properties files with the name 'application.properties'. 

One in GIT and the other one present locally. Which one will take precedence?

In order to ensure that the properties file from GIT takes higher priority,

it needs to be accessed first before the local properties file. For this, we use the bootstrap context.

Bootstrap context runs before the application context 

Spring Cloud creates a parent context to the spring application context called the ‘bootstrap’ context. 

This context takes precedence over the application context. This context is responsible for loading configuration details 

from an external source. Both these contexts share the Spring Environment, thus making configuration usage seamless.

Since the bootstrap context takes precedence, we have to mention the URI of the config-server in  bootstrap.properties


************************************************************************************
The config-server is contacted by the clients only once, during the start of the project. 

Therefore any changes made to the configuration after the application starts will not be reflected in the application.
*********************************************************************************************
Add these dependencies to all Microservices:

<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>Greenwich.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
</dependencyManagement>
<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
</dependency>


Create a bootstrap.properties file in each microservice.

spring.cloud.config.uri=http://localhost:1111
spring.datasource.username=root
spring.datasource.password=root
*******************************************************************************************************************************
 HANDLING AS CLIENTS ONLY CONTACT CONFIGSERVER ONCE:
 
 The Config Server is contacted by the microservices(Clients) Only Once during the start of the project.Therefore any changes after
 
 that will not be reflected in the application.
 
 
 *********************************************************************************************************************
 HANDLING WHEN CONFIG SERVER IS DOWN:
 
 
 if the cloud-config server is down, then the client will throw an error not during startup,
 
 but while trying to access the property at runtime. To avoid this we can have the failFast property set to true. 
 
 By this, the client(MicroService) will fail at startup time rather than at run time.
 
 spring.cloud.config.failFast=true

Also, in order to avoid config-server to be a single point of failure, we usually deploy multiple instances of it to ensure high 

availability. If the cloud config server is unavailable, it will use the properties files in the individual applications as a fallback.

If Config Server is Down Then This Property won't start the clients(Microservice).This is to be set at Microservices in BootStrap.prop
*************************************************************************************************************************************8
