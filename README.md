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
 
 
 
