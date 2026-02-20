# Microservices-Patterns-Chris-Richardson

 The high-level definition of microservice architecture (microservices) is an architectural style that functionally decomposes an application into a set of services.
 The microservice architecture uses services as the unit of modularity. A service has an API, which is an impermeable boundary that is difficult to violate.
 There are other benefits of using services as building blocks, including the ability to deploy and scale them independently. 
 
 <img width="1027" height="321" alt="image" src="https://github.com/user-attachments/assets/311ec8d1-7b30-4391-8462-2a53f8595a0d" />

 The microservice architecture has the following benefits:
 It enables the continuous delivery and deployment of large, complex applications.
 Services are small and easily maintained.
 Services are independently deployable.
 Services are independently scalable.
 The microservice architecture enables teams to be autonomous.
 It allows easy experimenting and adoption of new technologies.
 It has better fault isolation

 ### Decomposition guidelines

 SINGLE RESPONSIBILITY PRINCIPLE
 
 A class should have only one reason to change. 
 Each responsibility that a class has is a potential reason for that class to change. If a
 class has multiple responsibilities that change independently, the class won’t be stable.

 COMMON CLOSURE PRINCIPLE
 
 The classes in a package should be closed together against the same kinds of changes. A
 change that affects a package affects all the classes in that package

 The idea is that if two classes change in lockstep because of the same underlying reason, then they belong in the same package. Perhaps, for example, those classes implement a different aspect of a particular business rule. The goal is that when that
 business rule changes, developers only need to change code in a small number of
 packages (ideally only one).

 For more information about SRP, CCP, and the other OOD principles, see the article “The Principles of Object Oriented Design” on Bob Martin’s website (http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

 ###  Obstacles to decomposing an application into services

 On the surface, the strategy of creating a microservice architecture by defining services corresponding to business capabilities or subdomains looks straightforward. You
may, however, encounter several obstacles:
 Network latency
 Reduced availability due to synchronous communication
 Maintaining data consistency across services
 Obtaining a consistent view of the data

 A saga is a sequence of local transactions that are coordinated using messaging.

 ### Summary chapter 2
 - Architecture determines your application’s -ilities, including maintainability, testability, and deployability, which directly impact development velocity.
 - The microservice architecture is an architecture style that gives an application high maintainability, testability, and deployability.
 - Services in a microservice architecture are organized around business concerns—business capabilities or subdomains—rather than technical concerns.
 - There are two patterns for decomposition:
     – Decompose by business capability, which has its origins in business architecture
     – Decompose by subdomain, based on concepts from domain-driven design

 ### Interprocess communication in a microservice architecture

 #### Semantic versioning
 - MAJOR—When you make an incompatible change to the API
 - MINOR—When you make backward-compatible enhancements to the API
 - PATCH—When you make a backward-compatible bug fix

##### MAKING MINOR, BACKWARD-COMPATIBLE CHANGES
- Adding optional attributes to request
- Adding attributes to a response
- Adding new operations

##### MAKING MAJOR, BREAKING CHANGE

#### Message format
There are two main categories of message formats: text and binary
##### TEXT-BASED MESSAGE FORMAT
The first category is text-based formats such as JSON and XML. A downside of using a text-based messages format is that the messages tend to be verbose, especially XML. Every message has the overhead of containing the names of
the attributes in addition to their values. Another drawback is the overhead of parsing text, especially when messages are large. Consequently, if efficiency and performance are important, you may want to consider using a binary format. 
##### BINARY MESSAGE FORMATS
There are several different binary formats to choose from. Popular formats include Protocol Buffers (https://developers.google.com/protocol-buffers/docs/overview) and Avro (https://avro.apache.org).
#### Communicating using the synchronous Remote procedure invocation pattern
Figure 3.1 shows how RPI works. The business logic in the client invokes a proxy interface , implemented by an RPI proxy adapter class. The RPI proxy makes a request to
the service. The request is handled by an RPI server adapter class, which invokes the service’s business logic via an interface. It then sends back a reply to the RPI proxy, which returns the result to the client’s business logic.

Pattern: Remote procedure invocation
A client invokes a service using a synchronous, remote procedure invocation-basedprotocol, such as REST (http://microservices.io/patterns/communication-style/messaging.html)

<img width="926" height="616" alt="image" src="https://github.com/user-attachments/assets/d810b376-7105-4eed-b555-5f55d970dc65" />
##### Using REST
Today, it’s fashionable to develop APIs in the RESTful style (https://en.wikipedia.org/wiki/Representational_state_transfer). REST is an IPC mechanism that (almostalways) uses HTTP. Roy Fielding, the creator of REST, defines REST as follows:
REST provides a set of architectural constraints that, when applied as a whole, emphasizes scalability of component interactions, generality of interfaces, independent deployment ofcomponents, and intermediary components to reduce interaction latency, enforce security,
and encapsulate legacy systems.

##### THE REST MATURITY MODEL
Leonard Richardson (no relation to your author) defines a very useful maturity model
for REST (http://martinfowler.com/articles/richardsonMaturityModel.html) that consists of the following levels:
- Level 0—Clients of a level 0 service invoke the service by making HTTP POST requests to its sole URL endpoint. Each request specifies the action to perform, the target of the action (for example, the business object), and any parameters.
- Level 1—A level 1 service supports the idea of resources. To perform an action on a resource, a client makes a POST request that specifies the action to perform and any parameters.
- Level 2—A level 2 service uses HTTP verbs to perform actions: GET to retrieve, POST to create, and PUT to update. The request query parameters and body, if any, specify the actions' parameters. This enables services to use web infrastructure such as caching for GET requests.
- Level 3—The design of a level 3 service is based on the terribly named HATEOAS (Hypertext As The Engine Of Application State) principle. The basic idea is that the representation of a resource returned by a GET request contains links for performing actions on that resource. For example, a client can cancel an order using a link in the representation returned by the GET request that retrieved the order. The benefits of HATEOAS include no longer having to hard-wire URLs into client code (www.infoq.com/news/2009/04/hateoas-restful-api-advantages).

**BENEFITS AND DRAWBACKS OF REST**
- It’s simple and familiar.
- You can test an HTTP API from within a browser using, for example, the Postman plugin, or from the command line using curl (assuming JSON or some other text format is used).
- It directly supports request/response style communication.
- HTTP is, of course, firewall friendly.
- It doesn’t require an intermediate broker, which simplifies the system’s architecture.

**There are some drawbacks to using REST:**
- It only supports the request/response style of communication.
- Reduced availability. Because the client and service communicate directly without an intermediary to buffer messages, they must both be running for the duration of the exchange.
- Clients must know the locations (URLs) of the service instances(s). As described in section 3.2.4, this is a nontrivial problem in a modern application. Clients must use what is known as a service discovery mechanism to locate service instances.
- Fetching multiple resources in a single request is challenging.
- It’s sometimes difficult to map multiple update operations to HTTP verbs.

#### Using gRPC
You define your gRPC APIs using a Protocol Buffers-based IDL, which is Google’s language-neutral mechanism for serializing structured data. You use the Protocol Buffer compiler to generate client-side stubs and server-side skeletons. The compiler can generate code for a variety of languages, including Java, C#, NodeJS, and GoLang. Clients and servers exchange binary messages in the Protocol Buffers format using HTTP/2

A gRPC API consists of one or more services and request/response message definitions. A service definition is analogous to a Java interface and is a collection of strongly typed methods. As well as supporting simple request/response RPC, gRPC support
streaming RPC. A server can reply with a stream of messages to the client. Alternatively, a client can send a stream of messages to the server.

**gRPC has several benefits:**
- It’s straightforward to design an API that has a rich set of update operations.
- It has an efficient, compact IPC mechanism, especially when exchanging large messages.
- Bidirectional streaming enables both RPI and messaging styles of communication.
- It enables interoperability between clients and services written in a wide range of languages.
**gRPC also has several drawbacks:**
- It takes more work for JavaScript clients to consume gRPC-based API than REST/JSON-based APIs.
- Older firewalls might not support HTTP/2.

## Circuit breaker pattern
Pattern: Circuit breaker
An RPI proxy that immediately rejects invocations for a timeout period after the number of consecutive failures exceeds a specified threshold. See http://microservices.io/patterns/reliability/circuit-breaker.html.

**DEVELOPING ROBUST RPI PROXIES**

Whenever one service synchronously invokes another service, it should protect itself
using the approach described by Netflix (http://techblog.netflix.com/2012/02/faulttolerance-in-high-volume.html). This approach consists of a combination of the following mechanisms:
- Network timeouts—Never block indefinitely and always use timeouts when waiting for a response. Using timeouts ensures that resources are never tied up
indefinitely.
- Limiting the number of outstanding requests from a client to a service—Impose an upperbound on the number of outstanding requests that a client can make to a particular service. If the limit has been reached, it’s probably pointless to make
additional requests, and those attempts should fail immediately.
- Circuit breaker pattern—Track the number of successful and failed requests, and if the error rate exceeds some threshold, trip the circuit breaker so that
further attempts fail immediately. A large number of requests failing suggeststhat the service is unavailable and that sending more requests is pointless.
After a timeout period, the client should try again, and, if successful, close thecircuit breaker.

For example, the Polly library is popular in the .NET community (https://github.com/App-vNext/Polly). 

**Using service discovery**
Say you’re writing some code that invokes a service that has a REST API. In order to make a request, your code needs to know the network location (IP address and port) of a service instance. In a traditional application running on physical hardware, the
network locations of service instances are usually static. For example, your code could read the network locations from a configuration file that’s occasionally updated. But in a modern, cloud-based microservices application, it’s usually not that simple. As is
shown in figure 3.4, a modern application is much more dynamic.
 Service instances have dynamically assigned network locations. Moreover, the set of service instances changes dynamically because of autoscaling, failures, and upgrades. Consequently, your client code must use a service discovery.

 <img width="916" height="661" alt="image" src="https://github.com/user-attachments/assets/50608a99-f642-4520-9fb8-84700df8a52c" />

