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

