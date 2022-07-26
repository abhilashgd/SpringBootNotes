# SpringBootNotes

#1 What is spring boot ? why did you use spring boot in your project? why not spring ?

          What - 
          - spring boot is a module
          - is a framework for RAD build using spring framework with extra support of auto configuration and embedded application server (like tomcat, jetty)
          Why
          - it provides RAD
          How
          - it helps us in creating efficient, fast stand alone applications which you can just run. It basically removes a lot of configuraitons and dependencies. 

#2 what RAD?

          - RAD is modified waterfall model which focuses on developing software in a short span of time
          - phases of RAD are as follows
            - Business modeling - business model is designed for the product to be developed
            - data modeming - data model is designed, the relation between these data objects are established using info gathered in the first phase
            - process modeling -  process model is designed. Process description for adding, deleting, retrieving or modifying a data object are given
            - Application generation - the actual product is built using coding. Convert process and data model into actual prototypes
            - Testing and turnover - product is tested and if changes are required then whole process starts again.
            
#3 is this possible to change the port of embedded tomcat server in spring boot ?
          - Yes
          - by default 8080
          - server.port = 8090 in application.properties

#4 can we override or replace embedded tomcat server in spring boot?

          - yes
          - spring-boot-starter-web dependency is responsible for configuring tomcat to 8080
          - exclude spring-boot-starter-tomcat and add spring-boot-starter-jetty
          - we can replace the embedded tomcat with any other servers by using the starter dependencies like, You can use spring-boot-starter-jetty as a dependency for each project as you need.

#5 ca we disable the default web server in the spring boot application?

            The major strong point in spring is to provide flexibility to build your application loosely coupled. Spring provides features to disable the web server in a quick configuration. Yes, we can use the application.properties to configure the web application type. i.e spring.main.web-application-type=none.


#6 How to disable a specific auto-configuration class?
          You can use the exclude attribute of @EnableAutoConfiguration, if you find any specific auto-configuration classes that you do not want are being applied

          @EnableAutoCOnfiguration(exclude={SomeDataConfiguration.class})

          We can put , (comma) and add many classes to exclude.


#7 what does the @SpringBootApplication annotation do internally?
          As per the spring boot doc, the @SpringBootApplication is equivalent to using @COnfiguration, @EnableAutoConfiguration and @ComponentScan with their default attributes
          Spring boot enables the developer to use a single annotation instead of using multiple. But, as we know, spring provided loosely coupled features that we can use for each individual annotation as per our project needs


















