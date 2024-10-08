
# Architectural styles

## What is software architecture exactly?

[Len Bass and colleagues](https://books.google.it/books?id=-II73rBDXCYC&printsec=frontcover&redir_esc=y#v=onepage&q&f=false) defined it as: *The software architecture of a computing system is the set of structures needed to reason about the system, which comprise software elements, relations among them, and properties of both.*

[David Garlan and colleagues](https://books.google.it/books/about/Software_Architecture.html?id=fh_kjgEACAAJ&redir_esc=y) defined it as: *something that defines a family of such systems in terms of a pattern of structural organization. More specifically, an architectural style determines the vocabulary of components and connectors that can be used in instances of that style, together with a set of constraints on how they can be combined.*

In software, architecture styles can be classified into two main types: **monolithic** (single deployment unit of all code) and **distributed** (multiple deployment units connected through remote access protocols). 

**Monolithic**
* Layered architecture
* Microkernel architecture
* Modular monolithic architecture
* Big Ball of Mud

**Distributed**
* Service-based architecture
* Service-oriented architecture
* Space-based architecture 
* Event-driven architecture
* Microservices architecture

## Monolithic Architecture

Many small-to-medium web-based applications are built using a monolithic architectural style. In a monolithic architecture, an application is delivered as a single deployable software artifact. All of the UI, business, and database access logic are packaged together into a unique application and deployed to an application server. 

Although monolithic applications are sometimes described in negative terms by proponents of microservices architecture, these are often a great choice. Monoliths are easier to build and deploy than more complex architectures like n-tier or microservices. If your use case is well defined and unlikely to change, it can be a good decision to start with a monolith.

**When an application begins to increase in size and complexity, however, monoliths can become difficult to manage**. Each change to a monolith can have a cascading effect on other parts of the application, which may make it time consuming and expensive, especially in a production system. 

### Layered architecture

One common type of enterprise architecture is the multi-layered or n-tier architecture. With this design, an applications is divided into multiple layers, each with their own responsibilities and functions, such as UI, services, data, testing, and so forth.

![](images/swarch-layered.avif)

Benefits:
* N-tier applications offer good separation of concerns, making it possible to consider areas like UI (user interface), data, and business logic separately.
* It’s easy for teams to work independently on different components of n-tier applications.
* Because this is a well-understood enterprise architecture, it’s relatively easy to find skilled developers for n-tier projects.

Drawbacks:
* You must stop and restart the entire application when you want to make a change.
* Messages tend to pass up and down through the layers, which can be inefficient.
* Slow when changes are domain-based (you have to change all layers).
* Once it’s deployed, refactoring a large n-tier application can be difficult.

When to use:
* When developing simple, small applications, it is advisable to implement a layered architecture because it’s the simplest framework. 
* Can be used for applications that need to be built quickly because it’s easy to learn and implement. It is also good in cases where the developers do not have a lot of knowledge of software architectures or when they are undecided on which one to use.


| **Feature**      | **Score** |
|------------------|-----------|
| maintainability  | *         |
| testability      | *         |
| deployability    | *         |
| cost             | * * * * * |
| abstraction      | *         |
| scalability      | *         |
| elasticity       | *         |
| fault-tolerance  | *         |
| interoperability | *         |
| performance      | *         |
| evolvability     | *         |
| simplicity       | * * * * * |

### Modular Monolithic Architecture

![](images/swarch-modular-monolith.avif)

Instead of using layered architecture with horizontal logical layers, we can organize our code across vertical slices of business functionality.These slices are determined based on business demands, rather than enforced by technical constraints. When we add or change a feature in an application, our changes are scoped to the area of business concern not technical logical layers.

Modular monolithic architecture divides application logic into independent and isolated modules with business logic, database schema. Each module can follow their own logical separations, layered architecture style or clean architecture. Modules are represents Bounded Context of our application domain and we group features of Domain contexts in modules.

Modules can be a potential microservices when need to independently deployed and scale in the future refactorings our architecture.

Benefits:
* **Encapsulate Business Logic**: Business logics are encapsulated in Modules and it enables high reusability, while data remains consistent and communication patterns simple.
* **Reusable Codes, Easy to Refactor**: For large development teams, developing modular components of an application will increase reusability. Modular components can be reused that can help teams establish a single source of truth.
* **Better-Organized Dependencies**: With modular monoliths architecture, application dependencies will be more organized and visible. This will help developers to easily assess which parts of the application require which dependencies.
* **Better for teams**: Easier for developers to work on different parts of the code. with Modular Monolithic architecture, we can divide our developer teams effectively and implement business requirements with minimum affect to each other.

Drawbacks:
* **Can't diversifying technology**: Modular monoliths don't provide all benefits of microservices. If you need to diversifying technology and language choices, you can't do it with Modular Monolithic Architecture. These types of polyglot technology stacks can't use with Modular Monolithic Architecture.
* **Can't Scale and Deploy Independently**: Since the application is a single unit, it can't be scale separated parts or deploy independently like microservices. And this kind of applications has to move microservices due to reaching out scalability limits and also performance issues.

When to use:
* **Strict Consistency is Mandatory Cases**: For many companies unable to make the move to microservices, due to their database and data not appreciate for distributed architecture. For example if your application store high important data like debit on bank account, then you need strong data consistency that means your data should be correct for every time, if you got any exception you have to rollback immediately.
* **Modernization**: If you already have a big complex monolithic application running, the modular monolith is the perfect architecture to help you refactor your code to get ready for a potential microservices architecture. Instead of jumping into microservices, you can move modular monolithic without effecting your business and get benefits like speed up with a well-factored modular monolith.
* **Green Field Projects**: A modular monolith allow you to learn your domain and pivot your architecture much faster than a microservices architecture. You won't have to worry about things like Kubernetes and a services mesh at day 1. Your deployment topology will be drastically simplified.
* **Domain-oriented teams**

| **Feature**      | **Score** |
|------------------|-----------|
| maintainability  | * *       |
| testability      | * *       |
| deployability    | * *       |
| cost             | * * * * * |
| abstraction      | *         |
| scalability      | *         |
| elasticity       | *         |
| fault-tolerance  | *         |
| interoperability | *         |
| performance      | * * *     |
| evolvability     | *         |
| simplicity       | * * * * * |

### Microkernel architecture

![](images/swarch-microkernel.avif)

The microkernel architecture style (also referred to as the plug-in architecture) was coined several decades ago and is still widely used today. This architecture style is a natural fit for product-based applications (packaged and made available for download and installation as a single, monolithic deployment, typically installed on the customer’s site as a third-party product) but is widely used in many nonproduct custom business applications as well.

The microkernel architecture style is a relatively simple monolithic architecture consisting of two architecture components: a core system and plug-in components. Application logic is divided between independent plug-in components and the basic core system, providing extensibility, adaptability, and isolation of application features and custom processing logic.

Benefits:
* **Modularity and Flexibility:** It allows for easy addition, removal, or replacement of modules, making the system highly adaptable to changing requirements.
* **Simplified Maintenance and Debugging:** Issues are isolated to specific modules, simplifying debugging and maintenance activities, while updates can be performed independently.
* **Enhanced Security and Reliability:** By enforcing strict boundaries between modules, it reduces the attack surface and ensures graceful recovery from failures, enhancing system security and reliability.

Drawbacks:
* **Performance Overhead:** Microkernel architectures often incur a performance overhead due to the increased communication and context switching between modules running in user space and the microkernel. 
* **Complexity of Inter-Process Communication (IPC):** Communication between modules in a microkernel architecture typically involves inter-process communication (IPC) mechanisms such as message passing or remote procedure calls (RPC). 
* **Increased Development Complexity:** Designing and implementing a microkernel-based system can be more complex than traditional monolithic architectures. Developers need to carefully partition the system into modules, define clear interfaces and communication protocols, and manage dependencies between modules.

When to use:
* **Stable core**: Most of the changes are located within the plugins.
* **Configurability**: Plugins can be user for configuring the application in different ways easily.

| **Feature**      | **Score** |
|------------------|-----------|
| maintainability  | * * *     |
| testability      | * * *     |
| deployability    | * * *     |
| cost             | * * * * * |
| abstraction      | * * *     |
| scalability      | *         |
| elasticity       | *         |
| fault-tolerance  | *         |
| interoperability | * * *     |
| performance      | * * *     |
| evolvability     | * * *     |
| simplicity       | * * * * * |

### Big Ball of Mud
*A Big Ball of Mud is a haphazardly structured, sprawling, sloppy, duct-tape-and-baling-wire, spaghetti-code jungle. These systems show unmistakable signs of unregulated growth, and repeated, expedient repair. Information is shared promiscuously among distant elements of the system, often to the point where nearly all the important information becomes global or duplicated.*

*The overall structure of the system may never have been well defined. If it was, it may have eroded beyond recognition. Programmers with a shred of architectural sensibility shun these quagmires. Only those who are unconcerned about architecture, and, perhaps, are comfortable with the inertia of the day-to-day chore of patching the holes in these failing dikes, are content to work on such systems.*

*Brian Foote and Joseph Yoder*

![](images/swarch-big-ball-of-mud.avif)

Each dot on the perimeter of the circle represents a class, and each line represents connections between the classes, where bolder lines indicate stronger connections.


## Distributed Architecture

### Service-based architecture

![](images/swarch-service-based-architecture.avif)

When to use:
* Agility
* Time to market
* Fault-tolerance
* Monolithic data which is too difficult to separate

When not to use:
* Semantic coupling 
* Too many services

| **Feature**      | **Score** |
|------------------|-----------|
| maintainability  | * * * *   |
| testability      | * * * *   |
| deployability    | * * * *   |
| cost             | * * * *   |
| abstraction      | *         |
| scalability      | * * *     |
| elasticity       | * *       |
| fault-tolerance  | * * * *   |
| interoperability | * *       |
| performance      | * * *     |
| evolvability     | * * *     |
| simplicity       | * * *     |

### Service-oriented architecture

![](images/swarch-service-oriented-architecture.avif)

When to use:
* Abstraction
* Protocol-agnostic interoperability

When not to use:
* Tight budget
* Time constraints (shared components harms time to market) 

| **Feature**      | **Score** |
|------------------|-----------|
| maintainability  | *         |
| testability      | *         |
| deployability    | *         |
| cost             | *         |
| abstraction      | * * * * * |
| scalability      | * * *     |
| elasticity       | * * *     |
| fault-tolerance  | * * *     |
| interoperability | * * * * * |
| performance      | * *       |
| evolvability     | *         |
| simplicity       | *         |

### Event-driven architecture

![](images/swarch-event-driven-architecture.avif)

When to use:
* High performance systems (async)
* Highly elastic systems
* Fault tolerant systems

When not to use:
* Tight synchronization and consistency
* Time constraints (shared components harms time to market)
* Tight control on event ordering

| **Feature**      | **Score** |
|------------------|-----------|
| maintainability  | * * *     |
| testability      | * *       |
| deployability    | * * *     |
| cost             | * * *     |
| abstraction      | * * * *   |
| scalability      | * * * * * |
| elasticity       | * * * *   |
| fault-tolerance  | * * * * * |
| interoperability | * * *     |
| performance      | * * * * * |
| evolvability     | * * * * * |
| simplicity       | * *       |


### Microservices Architecture

![](images/swarch-microservices.avif)

A microservice is a small, loosely coupled, distributed service. Microservices let you take an extensive application and decompose it into easy-to-manage components with narrowly defined responsibilities. Microservices are built around business capabilities, are independently deployable by fully automated deployment process, and communicate over well-defined APIs. Services are owned by small, self-contained teams.

Benefits:
* **Agility, Innovation and Time-to-market**: Microservices architectures make applications easier to scale and faster to develop, enabling innovation and accelerating time-to-market for new features.
* **Flexible Scalability**: Microservices can be scaled independently, so you scale out sub-services that require less resources, without scaling out the entire application.
* **Small and separated code base**: Microservices are not sharing code or data stores with other services, it minimizes dependencies, and that makes easier to adding new features.
* **Easy Deployment**: Microservices enable continuous integration and continuous delivery, making it easy to try out new ideas and to roll back if something doesn’t work.
* **Technology agnostic, Right tool for the job**: Small teams can pick the technology that best fits their microservice and using a mix of technology stacks on their services.
* **Resilience and Fault isolation**: Microservices are fault tolerance and handle faults correctly for example by implementing retry and circuit breaking patterns.
* **Data isolation**: Databases are separated with each other according to microservices design. Easier to perform schema updates, because only a single database is affected.

Drawbacks:
* **Complexity**: Each service is simpler, but the entire system is more complex. Deployments and Communications can be complicated for hundreds of microservices.
* **Network problems and latency**: Microservice communicate with inter-service communication, we should manage network problems. Chain of services increase latency problems and become chatty API calls.
* **Development and testing**: Hard to develop and testing these E2E processes in microservices architectures if we compare to monolithic ones.
* **Data integrity**: Microservice has its own data persistence. Data consistency can be a challenge. Follow eventual consistency where possible.
* **Deployment**: Deployments are challenging. Require to invest in quite a lot of devops automation processes and tools. The complexity of microservices becomes overwhelming for human deployment.
* **Logging & Monitoring**: Distributed systems are required to centralized logs to bring everything together. Centralized view of the system to monitor sources of problems.
* **Debugging**: Debugging through local IDE isn’t an option anymore. It won’t work across dozens or hundreds of services.

When to Use:
* **Make Sure You Have a “Really Good Reason” for Implementing Microservices**: Check if your application can do without microservices. When your application requires agility to time-to-market with zero-down time deployments and updated independently that needs more flexibility.
* **Iterate With Small Changes and Keep the Single-Process Monolith as Your “Default”**: Sam Newman and Martin Fowler offers Monolithic-First approach. Single-process monolithic application comes with simple deployment topology. Iterate and refactor with turning a single module from the monolith into a microservices one by one.
* **Required to Independently Deploy New Functionality with Zero Downtime**: When an organization needs to make a change to functionality and deploy that functionality without affecting rest of the system.
* **Required to Independently Scale a Portion of Application**: Microservice has its own data persistence. Data consistency can be a challenge. Follow eventual consistency where possible.
* **Data Partitioning with different Database Technologies**: Microservices are extremely useful when an organization needs to store and scale data with different use cases. Teams can choose the appropriate technology for the services they will develop over time.
* **Autonomous Teams with Organizational Upgrade**: Microservices will help to evolve and upgrade your teams and organizations. Organizations need to distribute responsibility into teams, where each team makes decisions and develops software autonomously.

When NOT to Use:

* **Don’t do Distributed Monolith**: Make sure that you decompose your services properly and respecting the decoupling rule like applying bounded context and business capabilities principles. Distributed Monolith is the worst case because you increase complexity of your architecture without getting any benefit of microservices.
* **Don’t do microservices without DevOps or cloud services**: Microservices are embrace the distributed cloud-native approaches. And you can only maximize benefits of microservices with following these cloud-native principles.
  * CI/CD pipeline with devops automation
  * Proper deployment and monitoring tools
  * Managed cloud services to support your infrastructure
  * Key enabling technologies and tools like Containers, Docker, and Kubernetes
  * Following async communications using Messaging and event streaming services
* **Limited Team sizes, Small Teams**: If you don’t have a team size that cannot handle the microservice workloads, This will only result in the delay of delivery. For a small team, a microservice architecture can be hard to justify, because team is required just to handle the deployment and management of the microservices themselves.
* **Brand new products or startups**: If you are working on a new startup or brand new productDto which require significant change when developing and iterating your productDto, then you should not start with microservices. Microservices are so expensive when you re-design your business domains. Even if you do become successful enough to require a highly scalable architecture.

| **Feature**      | **Score** |
|------------------|-----------|
| maintainability  | * * * * * |
| testability      | * * * * * |
| deployability    | * * * * * |
| cost             | *         |
| abstraction      | *         |
| scalability      | * * * * * |
| elasticity       | * * * *   |
| fault-tolerance  | * * * * * |
| interoperability | * * *     |
| performance      | * *       |
| evolvability     | * * * * * |
| simplicity       | *         |

## Resources
- Fundamentals of Software Architectures (Chapters 9-)
- https://www.youtube.com/@markrichards5014/videos

