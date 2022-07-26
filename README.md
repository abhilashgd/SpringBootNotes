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


#8 how to use a property defined in application.properties file into your java class

          - use the @Value annotation to access the properties which is defined in the application - properties file

          EX: @Value ("${custom.value}"}
          - private String customVal;

#9 Explain @RestController annotation in Spring Boot

          - @RestController is a convenience annotation for creating Restful controllers. It is a specialisation of @Component and is autodetected through class path scanning. 
          It adds the @Controller and @ResponseBody annotations. It converts the response to JSON or XML.

          - which eliminates the need to annotate every request handling method of the controller class with the @ResponseBody annotation. It is typically used in combination with annotated handler methods based on the @RequestMapping annotation

          - indicates that the data returned by each method will be written into the response body instead of rendering a template

#10 difference between @RestController and @Controller ?

          - Its that the response from a web application is generally a view (HTML + CSS + JavaScript) because they are intended for human viewers while Rest API just return data in form of JSON or XML because most of the rest clients are programs.

          - same goes with @RestController and @cController annotation
          - @Controller Map of the model object to view or template and makes it human readable but @RestCOntroller simply returns the object and object data is directly written into HTTP response as JSON or XML.

#11 what are the major differences between RequestMapping and GetMapping?

          - RequestMapping can be used with GET, POST, PUT, and many other request methods using the method attribute on the annotation. Whereas GetMapping is only an extension of RequestMapping, which helps you to improve clarity on requests.

          Example:
          @RequestMapping ("/path") - handles get, post, put, etc

          @GetMapping("/path")

#12 what is the use of profiles in Spring Boot?

          - When developing applications for the enterprise, we typically deal with multiple environments such as Dev, QA and prod. The configuration properties for these environments are different.

          - for example, we might be using an embedded h2 database for dev, but prod could have the proprietary oracle DB2. Even if the DBMS is the same across environments, the URLs would definitely be different.

          - to make this easy and clean, spring has the provision of profiles. to help separate the configuration for each environment so that instead of maintaining this programatically, the properties can be kept in separate files such as application-dev.properties and application-prod.properties. the default application.properties points to the currently active profile using spring.profils.active so that the current configuration is picked up

#13 what is spring actuator ? What are its advantages ?

          - In spring boot, actuator is an additional feature that help you monitor and manage your application when you push it to production. These features include auditing, health and metrics gathering and many more features that can be automatically applied to your application.

          - you can enable this feature by adding the dependency: spring-boot-starter-actuaqtor in pom.xml

          - using spring actuator, you can access those flows like what bean is created, what is the CPU usage, http hits that your server has handled.

          Steps:
                    1. Add Dependency
                               <dependency>
                              <groupId>org.springframework.boot</groupId>
                                         <artifactId>spring-boot-starter-actuator</artifactId>
                              </dependency>

                    2. Hit localhost:8080/actuator
                              localhost:8080/actuator/health
                              localhost:8080/actuator/info
                    3. To expose 
                              in application.properties
                              management.endpoints.web.exposure.include =*
                              Exposes beans through localhost:8080/actuator/beans
                              Exposes env through localhost:8080/actuator/env
                              etc....
                              
#14 Enabling HTTP trace through actuator

                    before 2.2.x spring boot we can just add dependency and expose using : management.endpoints.web.exposure.include =*
                    but after 2.2.x it doesnt work because default-implementation stores the captured data in memory. Hence, it consumes much memory and memory is a pretty costly and precious. That is why this feature is now turned of by default and has to be turned on by the user explicitly if needed

                    to fix this issue just create the bean of HttpTraceRepository which is in memory repository. This will store the last 100 http request- response exchanges into your memory.
                    
                    CODE EXAMPLE: 
                    package com.code.decode.springBootExample;
                    import org.springframework.boot.actuate.trace.http.HttpTraceRepository;
                    import org.springframework.boot.actuate.trace.http.InMemoryHttpTraceRepository;
                    import org.springframework.context.annotation.Bean;
                    import org.springframework.context.annotation.Configuration;
                    @Configuration
                    public class ConfigurationClass
                    {
                    @Bean
                    public HttpTraceRepository|htttpTraceRepository())
                    {
                    return new InMemoryHttpTraceRepository():
                    }
                    }
                    
                    NOTE:
                    Instead of /actuator if we want our custom end point then add below line in application.properties

                    Management.endpoints.web.base-path =/manage
                    
#15 Customise the management server port

                    Add this to properties file
                              - management.server.port=8090

#16 How to create Custom end points

                    This can be achieved by adding the following annotations:
                    @Endpoint and @Component to class
                    @ReadOperation, @WriteOperation, or @DeleteOperation on method level
                    .
                    @ReadOperation maps to HTTP GET
                    @WriteOperation maps to HTTP POST
                    @DeleteOperation maps to HTTP DELETE
                    By adding @Bean annotated with @Endpoint, any methods annotated with @ReadOperation,
                    @WriteOperation, or @DeleteOperation are automatically exposed over JMX or HTTP.

                    package com.code.decode.springBootExample.controller
                    import org.springframework.boot.actuate.endpoint.annotation.Endpoint;
                    import org.springframework.boot.actuate.endpoint.annotation.ReadOperation;
                    import org.springframework.stereotype.Component;
                    @Component
                    @Endpoint (id = "customActuator")
                    public class CustomActuator {
                    @ReadOperation
                    public String currentDbDetails0) {
                    return
                    "Give current DB status of the application";
                    }
                    }


                    Main things 
                     @Endpoint (id = "customActuator")
                       @ReadOperation
                       
#17 Steps to deploy Spring boot web applications as JAR and WAR files?

                    - To deploy a spring boot web application, you just have to add the following plugin in the pom.xml file

                    <build>
                    <plugins>
                    <plugin>
                              <groupId> org.springframework.boot</groupId>
                              <artifactId> spring-boot-maven-plugin</artifactId>
                    </plugin>
                    </plugins>
                    <build>

                    By using the above plugin, you will get a JAR executing the package phase. This JAR will contain all the necessary libraries and dependencies required. It will also contain an embedded server. So, you can basically run the application like an ordinary JAR file.
                    Note: the packaging element in the pom.xml file must be set to jar to build a JAR file as below

                    <packaging>jar</packaging> or <packaging>war</packaging>
                    
#18 Advantages of YAML file over properties file

                    - more clarity and better readability
                    - perfect for hierarchical configuration data which is also represented in a better, more readable format
                    - support for maps, lists and scalar types

                    We need PropertySourceFactory with  YamlPropertiesFactoryBean(); to read yaml files in spring boot application

                    CODE example: 

                    package com.code.decode.yaml.demo;
                    import java.io.IOException;_

                    public class YamlPropertySourceFactory implements PropertySourceFactory{

                    @Override
                    public PropertySource<?› createPropertySource(String name, EncodedResource encodedResource)
                    throws I0Exception {

                    // we used the YamlPropertiesFactoryBean to convert the resources in YAML format
                    //to the java.util.Properties object.

                    YamlPropertiesFactoryBean factory = new YamlPropertiesFactoryBean();
                    factory.setResources(encodedResource.getResource());
                    Properties properties = factory.getObject();
                    return new PropertiesPropertySource(encodedResource-getResource()-getFilename(),properties);
                    }
                    }


                    package com.code.decode.yaml.demo. Controller;
                    import org.springframework.beans.factory.annotation.Value;!
                    @RestController
                    @ConfigurationProperties (prefix
                    "yaml")
                    @PropertySource (value="classpath: application.yml", factory = YamlPropertySourceFactory.class)

                    public class DemoController{
                    @Value("${spring.profiles.active}")
                    private String springProfiles;
                    @GetMapping("/pathToSomeAPI'
                    public void codeDecodeControllerDemo() {
                    System.out.println("springProfiles" + springProfiles);
                    }
                    }

























