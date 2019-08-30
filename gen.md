# General Notes

## MicroService Architecture 

- Small individual business functions 

- Advantages 
  - Independent development
  - Independent deployment
  - Speed & agility
  - Fault isolation
  - Mixed technology stack 
  - Granular scaling 
  
- Features 
  - Decoupling 
  - Modular 
  - Autonomy 
  - Continuous Delivery 
  - Responsibility 
  - Decentralized governance
  - Agility (easy to provision and deprovison)

- Why MicroServices
  - increased resilience (application is decentralized and decoupled as small services)
  - Scalability 
  - platform independent 
  - Faster time to market
  - CICD 
  - Easy debugging and maintenance
  
  
##Solution Arch

- Check first the app is already there 
- Who are the stockholders 
- Collect the attributes and requirement from business 
- High level flow diagram, actor, system 
- Use case diagrams
- Convert technical requirement 
- Low level design, define interface 
- Design 
  - What’s requirement 
  - What’s volume 
  - What’s the availability 
  - What’s the scalability 
  - What’s the load expected 
  - Choose technology, like spring boot 
  - Check all the available options 
  - Check approved options 
  - Check available skill set 
  - Check for best practice 
  - Which cloud 
  - Check the compatibility and evaluate 

## 15 factor app

- One code base, one application
- API first
- Dependency management
- Design, build, release, and run
- Configuration, credentials, and code
- Logs
- Disposability
- Backing services
- Environment parity
- Administrative processes
- Port binding
- Stateless processes
- Concurrency
- Telemetry
- Authentication and authorization

## Design patterns in spring 
- Provides interface, framework, ease my development work
- Inversion of control 
- Factory pattern 
- Proxy 
- Template 
- MVC 
- Singleton 
- Front Control 

## Spring Cloud 

- Service Registration and Discovery 
  - Netflix Eureka 
  - producer registered to eureka, client gets the producer instance from eureka by name 
- Load balancing
  - Ribbon
  - more then 1 producer registered with Eureka with different instance Id. 
  - Client gets the producer instance via ribbon.  
  - Ribbon contacts eureka and gives consumer instance in a round robin. 
- Circuit breaker or Fault tolerance 
  - Hystrix 
  - does fall back and fault tolerance.  
  - If the service is throwing exception, hystrix can call fallback method and return the default response.  
  - If the service fails continually, then hystrix call the fallback method directly to save latency.  
- Feign client
  - instead of ribbon and rest template, call directly via feign 
- Edge server 
  - Zuul
  - proxy server 
  - Api gateway
  - Route the traffic to service based on the context path, can do pre, post, error events
- Config server
- Security 
- Tracing 
  - Sleuth 
  - ELK 

## Microservices Pattern 
- 12 factor app - One codebase,API first,Dependency management,Design build release and run,Configuration, credentials, and code,Logs,Disposability,Backing services,Environment parity,Administrative processes,Port binding,Stateless processes,Concurrency,Telemetry,Authentication and authorization
- Monolithic 
- Microservice 
- Database per service 
- Shared Database
- Saga 
  - with DB per service 
  - if you want access the data from multiple DB, you can go with Saga. 
  - To maintain data consistency. 
  - Choreography based -  fire an event from 1 service to another service 
  - Orchestration based 
- API Composition
  - API composer fires the query to multiple microservices and collect that data and join the data in memory and sends the results 
  - ( similar to api gateway) 
- Domain Event
  - Publishes the event when there is a change in data.  
  - If you read only replica of you data, In case of change in data, to sync your replica the publish the events. 
- CQRS
  - Command query responsibility segregation
  - querying the data from multiple microservices
  - Similar to api composition.  But here it used event sourcing.  So can’t query the directly.  Here create read only replica of data.  Query the data from read only replica.  
  - Have domain event to update the replica if data changes. 
  - Command Layer - to publish the events to event store 
  - Query layer - query the read replica
- Api Gateway
  - Single entry point, service discovery, cross cutting concerns 
- Event publishing 
- Event sourcing
  - Persist the series of events, append the events in event store.
  - Interested party can subscribe the event source.
  - When ever the event is created it will be delivered to subscriber.  Order created, approved, shipped are the events.
  - Kafka
- Multiple service instance per host 
- Service instance per host 
- Service instance per vm 
- Service instance per container 
- Serverless
- BFF
- Client side discovery
  - client queries a service registry to discover the locations of service instances
- Server side discovery
  - router queries a service registry to discover the locations of service instances
- Service registry
  - A database of service instance locations
- Self registration
  - service instance registers itself with the service registry
- Circuit breaker 
- Microservices Chasis
  - framework that handles cross cutting concerns like externalizing configurations, logs, health checks, metrics, distributed tracing.  - Example chassis frame work are spring boot and cloud. 
- Externalize configurations 
- Remote Procedure Invocation
  - Communication between services
  - RPI based protocol
  -  REST 
- Messaging
  - Async messaging for HA
  - Apache Kafka, RabbitMQ
- Domain Specific Protocol
  - Email protocol such SMTP, IMAP
  - Media streaming protocol such as RTMP, HLS, and HDS
- Access Token
  - JWT
- Log Aggregation
  - Cloud Watch
  - Splunk
  - Kibana
- Audit logging
  - record user activities in audit table 
- Distributed Tracing
  - Assign each request with unique id and log the id in all services
  - Spring cloud sleuth
  - Zipkin server,  
- Health check api
  - Spring boot Actuator 

## 2 Phase Commit Protocol

- 1st phase
  - Voting
  - Coordinator sends commit voting to all the microservices.
  - Microservices performs all the actions till commit, if everything goes well acknowledge ok to coordinater. 
- 2nd phase
  - Commit
  - if coordinator receives ok  signal from all the microservices then issues commit signal.
  - If one of the microservices returns not ok then issues rollback signal.  

## Architectural

- Program to Interface not the implementations
- CI
  - Always merge the code the master often to avoid integration issues 
- CD
  - Delivery
  - run automation test cases and deploy to prod with single click
- CD
  - Deployment
  - Automatically deployed to PROD

## SOLID Principles

- S
  - Single Responsibility model 
  - class should have only one job
- O
  - Open closed principle
  - Objects or Entities should be open for extension and closed for modification 
- L
  - Liskov Substitution principle
  - if q(x) is provable of objects of x of type T, then q(y) should be provable of objects of y of type S, where S is subtype of T
- I
 - Interface Segregation principle
 - client should never forced to implement an interface that it doesn't use 
- D
 - Dependency Inversion principle
 - Entities must depends on the abstractions and not on concretions

## BASE
- No Sql 
- B
 - Basically available 
- S
 - Soft state
 - state of the system will be changing even if no inputs 
- E
 - Eventual consistency
 - Data will be consistent over a period of time

## CAP theorem or Brewer theorem 
- Distributed data store can’t guarantee the 3 characteristics.
- They can guarantee only 2 out of 3 
- C
  - Consistency
  - Every read receives most updated writes or error 
- A
  - Availability
  - System should always sends the response either success or failure 
- P
  - Partition tolerant
  - System should continuously operate even if lose if data or part of system failure 

## ACID
- DBMS 
- A
 - Atomicity
 - Transactions with bunch statements, either all success or complete rollback.
 - No partial commit 
- C
 - Consistency
- I
 - Isolation
 - Multiple threads accessing the same data it should be isolated.
 - Lock the table for concurrent update 
- D
  - Durability
  - Once the data committed it should be available even if the power failure 

## Container to Container network 

- Also called C2C
- To restrict the access to micro service 
- If the services are running in same container 
- Via cloud foundry, overlay IP address 
- Remove the public route of the microservice 
- add-network-policy source app, destination app, port and protocol (tcp)
- Get the IP address of destination service and cal from source service to destination via ip and port configured 
- Destination service can be configured in service discovery 
- CF service discovery has been deprecated and it’s moved to CF Networking Release 

## Spring Boot 

- Transactions - 2 PC, XA standard (JTA compatible), Eventual consistency and Compensation translations 
- Web api 
- Service Api 
- Spring cloud - Tracing
- Sleuth - generates trace id and span id and attach it to the request header
- Zipkin - Storing and Visualizing traces 
- Logging 
- Loggrrgator 
- ELK -  Elastic Search,  Log Stash and Kibana
- Elastic search - Search and Analyze 
- Log stash -  Collect and Transform
- Kibana - Visualize (similler to splunk)
- Api gateway 
- Swagger 
- DB versioning 
- Flyway open source tool 
- Security - TLS1.2, SSL, JWT OAuth2, API Gateway, DDOS
- 7 layers of security
- Apex 
- Spring profiles 
- Spring Devtools - Auto configuation, Auto restart, remote debugging
- Actuator 
- Deploy but don’t run


## Cloud Foundry 
- its PAAS
- Build Pack - framework of runtime, it automates the detection of application framework 
- BOSH - creates and deployed VM and deploys Cloud foundry on top 
- Blobstore - repo of large binary files (github cant store them). it has application code packages, buildpacks and droplets
- Diego -self healing container management system

## UML
- Unified Modeling Language 
- Graphical , Visual representation of Artifact 
- Structural - static part of the model - class, interface, component, node - class diagram, component diagram, deployment diagram
- Behavioral - dynamic part of model - message , state, dependency, association - use case diagram, sequence diagram, state diagram, activity diagram 
- Architectural  - candidates architecture 
- views - use case view, design view, deployment view 
- Use case diagram 
- class diagram 
- object diagram 
- State diagram 
- sequence diagram 
- activity diagram 
- Deployment diagram 
- Components - Actor, Object
- Deployment Diagram - Nodes, Dependencies, Components, Links 
- state diagram - initial state, transition, final state 
- Dependency - dotted arrow from dependent side to independent side.  Inheritance
- Association - dotted 2 side pointing arrow 
- Message - pointing arrow 
- class notation - square box with 4 parts - 1. Name 2. Instance variables 3.  Methods 4.  Additional responsibilities (optional)
- Object notation - same as class.  But name is underlined 


## Domain Driven Design 
- software methodology for microservices 
- Way of looking at software from top down. 
- Development should focus on business 
- Focus on design
- Not focus on technology 
- Convert your complex business functions into Domain 
- Spilt the Domain to sub domains 
- Boundary context between sub domains 
-  Strategic design 
    - Everything you talk in context 
    - bounded context 
    - Context map 
    - ubiquitous language 
    - Continues integration 
- if your building the house 
   - choose the the type of the house - flat, form house, villa 
   - If you choose flat as the house, flat is the domain 
   - Flat has entry gate, garden, living hall, kitchen etc - these are sub domains 
   - Let’s take sub domain living hall.  Drawing layout plan of living hall is domain model (object) 
   - Layer around sub domains are boundary context 
   - Connecting the boundary contexts are context map. or relation between boundary contexts 
   - Internal terms of living hall is ubiquitous language 
- Ubiquitous Language 
   - developers, SME, architects, testers, application, code all should use same terms (not tech terms, testers will not understand) is Ubiquitous language 
- Bounded context 
   - each sub domain would have its own bounded context 
   - Each bounded context has its own domain model, ubiquitous language, api, Db, documentation 
- Tactical design
- Talks about the implementation 
- Components inside the bounded context 
- Tends to change
- Entities, Services, Factories, Repositories, VO
- Value Objects - String is value object.  It’s helps handle character array, Immutable, Thread safe 
- Aggregates - one to many relation between entities 

- Event Storm 
   - like Brainstorming
   - Event Storm sessions 
   - Bring the right peoples ( domain experts, tech experts) 
   - Use sticky notes and colour code the events. ( roles, policy, processes, errors) 
   - start write the events in sequence order in white board 
   - Identify the bounded context from sequence of events 
   - 


## Java Design Patterns 

- Creational Patterns
   - Singleton
   - Factory / Template 
   - Abstract Factory - Factory of Factories 
   - Builder pattern - instantiate the classes which has larger or complex fields step by step.   Instantiating with argument constructor with many fields is complex.  Instead include static builder class and build. (Private constructer) 
   - Prototype pattern - to minimize the cost of object creation - copying the object once it’s created.  And reuse the copy of the request for future requests - clone 
- Structural Patterns 
   - Adapter - adapts one interface to another.  Acts as bridge between two unrelated interfaces. Example , 1 interface Builder has a method build(type, location), 2nd interface Advanced Builder has 2 methods, buildFlat(location) and buildFarmHouse(location).  Now adapter can be used to use interface 2 in interface 1 - Arrays.asList() coverts arrays into List and marshal and unmarshalling
   - Bridge - segregates abstract classes from its implementation and act as bridge between them - 1 interface Shape 1 interface colour.   Combining this 2, RedCircle, RedSqure, BlueCircle 
   - Filter Pattern - filter through set of objects with different criteria - Hibernate criteria 
   - composite - when we want to treat the group of objects in a similar manner - Employee  class has variables of name, age, role and coworkers list.  Coworker is the type of employee class 
   - Decorator -Dynamically adding responsibilities to a class in a run time 
   - Facade - provides the top level interface to clients to access the system.  They don’t have to know the internal logic. 
   - Flyweight - reduces strain on JVM and it’s memory.  If application needs to create same instances multiple times, use pool of instances and reuse it.  Connection Pool.  String in java are examples 
   - Proxy - when we want limit the capabilities and functionalities of class, use another class as proxy and restrict it 
- Behavioral Patterns 
   - interpret - interprets the inputs and gives the result.  Java compiler, google translate is the example.  If I pass string to api, it interprets the string by splitting the string into more then one string if it had delimiter comma, and returns string array 
   - template - abstract class pre defined the ways to run its methods.  Sub classes should follow the way defined in the abstract class - employee abstract class has abstract method work.  Subclass developer implements work() and it’s about writing code.  Manager subclass implements work() it’s about managing developers.  Here work is the template all the employees are following. Abstract method of abstractSet, AbstractList.  
   - Chain of responsibility - method 1 calls method 2, 2 calls 3, and 3 calls 4- if any exceptions occurs in m4 it would be transferred to 3, 2 and 1 if no try catch or approval process 
   - Command - 2 phase commit, runnable 
   - Iterator - collections 
   - Mediator - mediator between 2 different objects which communicate any way. Messaging system 
   - memento - concerned with pervious state of the object. When we want to store the state of an object so that we can restore at any time, Event sourcing 
   - observer - to monitor the state of the object.  One to many relationship, changing state of one class may impact others classes.  So system should monitor state of the object 
   - Strategy - Algorithm or class behavior is dynamic.  It can changed in runtime.  Comparator.compare, Collections.sort. 
   - visitor - move the business logic from each individual element of a group to different class. 

## J2EE Patterns
- MVC Pattern 
- Business Delegate - decouples business layer from presentation later.  Delegates the requests to different business based in the request type.  Requests comes business delegate.  Delegate checks the type of the requests, if it’s type customer sends to customer service, if it’s type payments sends to payments service 
- DAO 
- Front Controller 


## Hibernate 
- ORM 
- Configuration, Session Factory, Session, Transaction, Query, Criteria 
- First level cache, session cache, mandatory, objects are stored here before persist 
- cache query, second level cache, associated with session factory, optional, queries that frequently holding same set of parameters 
- Transient state - state which is not persisted 
- Lazy loading - child objects are not loaded when the parent is loaded 
- Advantages of using hibernate template - don’t have to close the sessions.  Frameworks takes care if it.  Exception handling easier 
- Save vs persist - save returns serializable object , persist is void 
- load vs get - load throws exception if the records not found. Get returns null 
- Update vs Merge - update can be used only with in the session.  If session is closed update would throw the error.  If we don’t know the state of the session use merge.  Merge can be used at anytime. 
- Transient - object  is created with no primary and not associated wit the session 
- Persistent - saved the object to DB or object is retrieved from DB 
- Detached - object is created but session is closed before persisting 
- detached objects comes to persistent state if lock() or update () is called 
- By default class is mutable.  To make immutable, Mark a class as mutable = false 
- Dirty checking is automate.  If the any change in object only then it hits the Db 


## Java8 
- Lambda expressions - gives ability to pass a functionality as a method argument - helps to reduce code clutter and better readability 
- pipelines and streams  - iteration, filter, Match 
- date and time api 
- default methods 
- Static methods in interface 
- concurrent api 
- Parallel - iterating over collection can be parallel 
- perm gen space removed 
- For each method 
- Type annotations - @NotNull @ReadOnly 
- Optional 
- method reference - alternate for lambda for better readability, 2 colon , ::
  
   
  
  
  
