Notes for the book Clean Architecture by Robert C. Martin

# Clean Architecture

The goal of good software design is to minimize the human resources required to build & maintain the required system.

Making messes is always slower than staying clean.

The only way to go fast is to go well.

The first value of software is its behavior.<br>
The second value is its structure.

Architectures should be as shape agnostic are practical.<br>
Behavior is urgent but not always particularly important.<br>
Architecture is important but never particularly urgent.

1. Urgent & important
2. Not urgent & important
3. Urgent & not important
4. Not urgent & not important

As a software developer, you are a stakeholder.

<br>

> ## **Paradigms**

Paradigms are ways of programming, relatively unrelated to language.

3 Programming Paradigms:
1. Structured Programming
    - Imposes discipline on direct transfer of control
2. Object Oriented Programming
    - Imposes discipline on indirect transfer of control
3. Functional Programming
    - Imposes discipline upon assignment

All programs can be constructed from:
- Sequence
- Selection
- Iteration

Testing shows the presence, not the abscence of bugs.

Object oriented:<br>
X - Encapsulation<br>
X - Inheritance<br>
Y - Polymorphism

Our programs should be device independent.

Polymorphism means that any source code dependency, no matter where it is, can be inverted. 
- Independent deployability
- Independent developability

Variables in functional languages do not vary.

All race conditions, deadlock conditions, & concurrent update problems are due to mutable variables.

Immutability is practical if you have infinite storage & infinite processor speed.

One compromise is to segregate the app into mutable & immutable components.

Use transactional memory to protect the mutable variables from concurrent updates & race conditions.

Event sourcing is a strategy wherein we store the transactions but not the state. When state is required, we simply apply all the transactions from the beginning of time.

Each paradigm takes something away from us. None of them has added to our capabilities. Software is composed of sequence, selection, iteration, and indirection.

<br>

> ## ***SOLID***

The goal of ***SOLID*** is the creation of mid-level software structures that:
1. Tolerate change
2. Are easy to understand
3. Are the basis of components that are to be used in many software systems.

> *Single Responsibility Principle* (*SRP*)

Definition:

- The best structure for a software system is heavily influenced by the social structure of the organization that uses it so that each software module has one and only one reason to change.

A module (a cohesive set of functions & data structures) should be responsible to one and only actor (user or stakeholder).

Symptoms of violating it:
- accidental duplication
    - separate code that different actors depend on
- merges

Solutions:
- Facade pattern

At the level of components, this is called the **Common Closure Principle**.
At architectural level, it's **Axis of Change** responsible for the creation of **Architectural Boundaries**.

> *Open Closed Principle* (*OCP*)

Defintion:

- For software systems to be easy to change they must be designed to allow the behavior fo those systems to be changed by adding new code, rather than changing existing code.

A software artifact should be open for extension but closed for modification.

The highest level policy deals with the central concern.

The goal is to make the system easy to extend without incurring a high impact of change. This goal is accomplished by partitioning the system into components and arranging those components into a dependency hierarchy that protects higher-level components from changes in lower-level components.

> *Liskov Substitution Principle* (*LSP*)

Definition:

- To build software systems from interchangeable parts, those parts must adhere to a contract that allows those parts to be substituted one for another.

Applicable because there are users who depend on well defined interfaces and on the substitutability of the implementations of those interfaces.

> *Interface Segregation Principle* (*ISP*)

Definition:

- Avoid depending on things you don't use.

Dynamically typed languages create systems that are more flexible & less tightly coupled than statically typed languages.

Depending on something that carries baggage that you don't need can cause you troubles that you didn't expect.

> *Dependency Inversion Principle* (*DIP*)

Definition:

- The code that implements high level policy should not depend on the code that implements low level details. Rather, details should depend on policies.

The most flexible systems are those in which source code dependencies refer only to abstractions, not to concretions.

It is the volatile concrete elements of our system that we want to avoid depending on interfaces are less volatile than implementations.
- Don't refer to volatile concrete classes. Refer to abstract interfaces instead. This generally enforces the use of abstract factories.
- Don't derive from volatile concrete classes.
- Don't override concrete functions. Concrete functions often require source code dependencies. To manage these dependencies, you should make the function abstract & create multiple implementations.
- Never mention the name of anything concrete and volatile.

Use abstract factory patterns to manage dependencies.

<br>

> ## **Components**

Components are the unit of deployment.

Programs will grow to fill all available compile and link time.

> *The Reuse/Release Equivalence Principle* (*REP*)

The granule of reuse is the granule of release.

Classes & modules that are grouped together into a component should be releaseable together.

> *The Common Closure Principle* (*CCP*)

Gather into component those classes that change for the same reasons & at the same times separate into different components those classes that change at different times & for different reasons.

A component should not have multiple reasons to change.

> *The Common Reuse Principle* (*CRP*)

Don't force users of a component to depend on things they don't need.

Classes that are not tightly bound to each other should not be in the same component.

> *More Principles*

1. Acyclic Dependencies Principle:<br>
Allow no cycles in the component dependency graph.<br>
Break cycles with *DIP* or creating a new component.<br>
Dependency diagrams have little to do with the function of the app. They are a map to the buildability & maintainability of the app.

2. Stable Dependencies Principle:<br>
A component with lots of incoming dependencies is very stable because it requires a great deal of work to reconcile any changes with all the dependent components.

    Stability metrics:

    ```
    Fan in = incoming dependencies
    Fan out = outgoing dependencies

    Instability: I = (fan out)/(fan out + fan in)

    [0,1] where
    0 = maximally stable
    1 = minimally stable
    ```

    The ```I``` metric of a component should be larger than the ```I``` metrics of the components that it depends on<br>
    ```I``` metrics should decrease in the direction of dependency<br>
    You can usually count import statements to determine ```I```<br>
    Some components should be stable and some unstable

3. Stable Abstractions Principle:<br>
A component should be as abstract as it is stable<br>
Dependencies run in the direction of abstraction

    ```
    Nc = # of classes in a component
    Na = # of abstract classes & interfaces in the component

    Abstractness: A = Na/Nc

    [0,1] where
    0 = no abstract classes at all
    1 = component contains nothing but abstract classes
    ```

    ```
    The most desirable position for a component is at one of the two endpoints of the Main Sequence

    Distance from the Main Sequence:
    D = |A + I -1|

    [0,1] where
    0 = on the Main Sequence
    1 = as far away as possible from the Main Sequence
    ```

    The dependency management metrics measure the conformance of a design to a pattern of dependency & abstraction that I (Robert C. Martin) think is a good pattern.

<br>

> ## **Architecture**

The strategy behind the facilitation is to leave as many options open as possible, for as long as possible.

Good architecture makes the system easy to understand, easy to develop, easy to maintain, & easy to deploy. The ultimate goal is to minimize the lifetime cost of the system & to maximize programmer productivity.

Software is policy & details. The policy embodies all the business rules & procedures. The policy is where the value of the system lives.

The details are those things that are necessary to enable humans to communicate with the policy but that don't impact the behavior of the policy at all.

Delay and defer --> As you go you will have more info with which to make decisions.

A good archtect maximizes the # of decisions not made.

Any org that designs a system will produce a design whose structure is a copy of the organization's communication structure.

> *Decoupling*

Decoupling types:
- Source level
- Deployment level
- Service level

A good architecture should start as a monolith, go into services, & be able to go back to monolith.

The database is a tool the business can use indirectly.

Boundaries are drawn when there is an Axis of Change.

Software systems are statements of policy.

Level = the distance from the inputs & the outputs.

The farther a policy is from both the inputs & outputs of the system, the higher its level we want source code dependencies to be decoupled from data flow & coupled to level.

> *Business Rules*

Business rules are rules or procedures that make or save the business money.

Critical business rules would exist even if there was no system to automate them.

Critical business data would exist even if the data were not automated.

Critical rules & data are bound so they are a candidate for an entity.

An entity embodies a small set of critical business rules operating on critical business data.

Some business rules make or save money for the business by defining & constraining the way that an automated system operates. These rules would not be used in a manual environment.

> *Use Cases*

A use case is a description of the way that an automated system is used. It specifies the input to be provided by the user, the output to be returned to the user, and the processing steps involved in producing that output.

Use cases describe the application specific rules that govern the interaction between the users & the entities.

A use case is an object.

Use cases depend on entities; entities do not depend on use cases.

Use request & response models. They should know nothing of your system.

> *Frameworks*

Architectures are not abstract frameworks.

Good architectures are centered on use cases so that architects can safely describe the structures that support those use cases without committing to tools, frameworks, environments.

Frameworks are options to be left open.

The web is a delivery mechanism tool & your app architecture should treat it as such.

Look at each framework with a jaded eye. View it skeptically.

You should be able to unit test all use cases without any of the frameworks in place.

<br>

> ## **Clean Architecture**

- Independent of framework
- Testable
- Independent of the UI
- Independent of the database
- Independent of any external agency

[Clean Architecture diagram](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

The outer circles are mechanisms. The inner circles are policies.

The Dependency Rule: source code dependencies must point only inward, toward higher level policies.

Entities enterprise-wide critical business rules.

The Humble Object Pattern is a design pattern that was originally identified as a way to help unit testers to separate behaviors that are hard to test from behaviors that are easy to test.

The view is the humble object that is hard to test.

The presenter is the testable object.

Testability is an attribute of good architectures.

There is no such thing as an ORM. Objects are not data structures.

ORMs would be better named "data mappers" because they load data into data structures from relational database tables.

MAIN is a dirty low level module in the outermost circle of the clean architecture.

> *SOA*

Service Oriented Architecture (SOA) has become popular because:
1. Services seem to be strongly decoupled from each other. This is only partially true
2. Services appear to support independent of development & deployment. This is only partially true

Services, in and of themselves, do not define architecture.

Decoupling Fallacy - still tightly bound<br>
Fallacy of independent development and deployment - still coupled so it's hard to do

Architectural boundaries do not fall between services. The run through the services, dividing them into components.

> *Testing*

Tests are a part of the system. Their rule is to support development, not operation.

Design for testability. Don't depend on volatile things.

The goal is to decouple the structure of the tests from the structure of the application.

> *Miscellaneous*

Although software does not wear out, it can be destroyed from within by unmanaged dependencies on firmware & hardware.

Three activities in building software:
1. Make it work
2. Then make it right
3. Then make it fast

Target Hardware Bottlneck

[HAL](https://en.wikipedia.org/wiki/Hardware_abstraction) & [OSAL](https://en.wikipedia.org/wiki/Operating_system_abstraction_layer)

<br>

> ## **Details**

The database is a detail.<br>
The web is a detail (GUI also).<br>
Frameworks are details (Don't marry the framework)
- you can use a framework; just don't couple to it

<br>

> ## **The Missing Chapter**

- Package by layer
- Package by feature
- Ports & Adapters
- Package by Component

Controller to repo is called *Relaxed Layered Architecture*.

Component = a grouping of related functionality behind a nice clean interface which resides inside an execution environment like an application

Benefit - there's just one place to go to change things

Don't make everything ```public```. It violates encapsulation.

The fewer ```public``` types you have, the smaller the number of potential dependencies.

Dependencies can also be decoupled at the source code level, by splitting code across different source code trees.
- you could have domain code & infrastructure code

The devil is in the implementation details.


