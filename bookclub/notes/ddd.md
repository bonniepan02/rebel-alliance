# Domain Driven Design

## Chapter 4

### Layered Architecture

Creating programs that can handle very complex tasks calls for seperation of concerns, allowing concerntration on different parts of the design in isolation.

Partition a complex program into layers. Develop a design within each layer that is cohesive and that depends only on the layers below.

Application Layer: This layer is kept thin. It does not contain business rules or knowledge, but only coordinates tasks and delegates work to collaberations of domain objects in the next layer down. It does not have state refelcting the business situation, but it can have state that reflects the progress of a task for the user or the program.

Isolated layers are much less expensive to maintain, because they tend to evolve at different rates and respond to different needs.

The infrastructure layer usaully does not initiate action in the domain layer. Being "below" the domain layer, it should have no specific knowledge of the domain it is serving. Such technical capabilities are most often offered as Services. The main benefit is simplifying the application layer: knowing when to send a message, but not burdened with how.

The bottom line is this: If the architecture isolates the domain-related code in a way that allows a cohesive domain design loosely coupled ti the rest of the system, then that architecture can probably support domain driven design.

## Chapter 5

- Entity: an object represent something with continuity and identity(something tracked through different states and across different implementations)
- Value Object: an attribute that describes the state of something else.
- Service: Actions or operations, rather than objects. It does not correspond with state.

### Associations

For every traversable association in the model, there is a mechanism in the software with the same properties.
There are at least 3 ways of making associations more tractable

- imposing a traversal direction
- adding a qualifier, effectively reducing multiplicity (qualifier makes realtionship from one to many to one to one; or many to many to one to many thus simplify the design, also gives significance to the remaining bidirectional associations.)
- eliminating nonessential associations

It is important to constrain relationships as much as possible. It reduces interdependence and simplifies the design. This refinement actually reflects insight into the domain as well as making a more practical design. It captures the understanding that one direction of the association is much more meaningful and important than the other.

### Entities

Many objects are not fundamentally defined by their attributes, but rather by a thread of continuity and identity. They represent a thread of identitiy that runs through time and often cross distinct representations. They have life cycles that can radically change their form and content, but a thread of continuity must be maintained.

### Value Objects

Many objects have no conceptual identity. These objects describe some characteristic of a thing.

Value objects are instantiated to represent elements of the design that we care about only for what they are, not who or which they are.

Value objects can reference Entities. Value objects are often passed as parameters in messages between objects. They are frequently transient, created for an operation and then discarded.

When you care only about the attributes of an element of the model, classify it as a VALUE Object. Make it express the meaning of the attribtues it conveys and gives it related functionality. Treat the value object immutable. Don't give it any identity and avoid the design complexities necessary to maitain Entites.

Note: A value object should always override .equals() in Java. (Remember to override .hashCode() as well.)

Note: Immutability is a great simplifier in an implementation, making sharing and reference passing safe. There are cases when performance consideration will favor allowing a value object to be mutable, e.g., the value changes frequently, creation or deltion is expensive, replacement will disturb clustering, there is not much sharing of values. Just to re-iterate: If a value's implementation is to be mutable, then it must not be shared. Design value objects as immutable when you can.

### Services

Services are abstraction of activities or actions.

Complex operations can easily swamp a simple object, obscuring its role. And because these operations often draw together many domain objects, coordinating them and putting them into action, the added responsibility will create dependencies on all those objects, tangling concepts that could be understood independently.

A service is an operation offered as an interface that stands alone in the model, without encapsulating state, it emphasizes the relationship with other objects, a verb rather than a noun. Operation names should come from the ubiquitous language or be introduced into it. Parameters and results should be domain objects.

A good service has three characteristics.

1. The operation relates to a domain concpet that is not a natual part of an Entity or Value object.
2. The interface is defined in terms of other elements of the domain model.
3. The operation is stateless.

#### Services and the Isolated Domain Layer

Services are not used only in the domain layer. It takes care to distinguish services that belong to the domain layer from those of other layers, and to factor responsilities to keep that distinction sharp.

Service pattern is also valuable as a means of controlling granularity in the interfaces of the domain layer as well as decoupling clients from the entities and value objects.

Medium-grained, stateless services can be easier to reuse in large systems because they encapsulate significant functionality behind a simple interface. Also fine-grained objects can lead to inefficient messaging in a distributed system.

This pattern favors interface simplicity over client control and versatility.

#### Modules

There is a limit to how many things a person can think about at once(hence low coupling). Incoherent fragments of ideas are as hard to understand as an undifferentiated soup of ideas(hence high cohesion). Lowcoupling and high cohesion are general design principles.

Like everything else in a domain-driven design, Modules are a communications mechanism. If your model is telling a story, the Modules are chapters. Give the modules names that become part of the ubiquuitous language. Modules and their names should reflect insight into the domain.

## Chapter 6

The challenges of managing the life cycle of a domain object:

1. Maintaining integrity throughout the life cycle
2. Preventing the model from getting swamped by the complexity of managing the life cycle

This chapter addresses these issues through the following three patterns:

### Aggregates

An aggregate tighten up the model itself by defining clear ownership and boundaries, avoiding a chaotic, tnangled web of objects. It is a cluster of associated objects that we treat as a unit for the purpose of data changes. Each aggregate has a root and a boundary. The boundary defines what is inside the Aggregate. The root is a single, specific Entity contained in the Aggregate. The root is the only member of the Aggregate that outside objects are allowed to hold references to, although objects within the boundary may hold references to each other.

### Factories

Creation of an object can be a major operation in itself, but complex assembly operations do not fit the responsibility of the created objects. Combining such responsibilities can produce ungainly designs that are hard to understand. Making the client direct construction muddies the design of the client, breaches encapsulation of the assembled object of AGGREGATE, and overly couples the client to the implementation of the created object. Provide an interface that encapsulates all complex assembly and that does not require the client to reference the concrete classes of the objects being instantiated.

#### when a constructor is all you need

Factories can obscure simple objects that don't use polymorphism. The trade offs favor a bare, public constructor in the following circumstances.

- The class is the type. It is not part of any interesting hierarchy, and it isn't used polymorphically by implementing an interface.
- The client cares about the implementation, perhaps as a way of choosing a strategy.
- All of the attributes of the object are available to the client, so that no object creation gets nested inside the constructor exposed to the client.
- The construction is not complicated.
- A public constructor must follow the same rules as Factory: It must be an atomic operation that satisfies all invariants of the created object.

To sum up, the access points for creation of instances must be identified, and their scope must be defined explicitly. They may simply be constructors, but often there is a need for a more abstract or elaborate instance creation mechanism. This need introduces new constructs into the design: Factories.

Factories usaully do not express any part of the model, yet they are a part of the domain design that helps keep the model expressing objects sharp. A Factory encapsualtes the life cycle transitions of creation and reconstitution.

### Repositories

Another transition that exposes technical complexity that swamp the domain design is the transition to and from storage. This transition is the responsibility of Repository.

The repository pattern is a simple conceptual framework to encapsulate technology of data retrieval and bring back our model focus. The problem it is trying to solve is:

A subset of persistent objects must be globally accesible through a search based on object attributes. Such access is needed for the roots of Aggregates that are not convenient to reach by traversal. They are usally entities, sometimes value objects with complex internal structure, and sometimes enumerated values. Providing access to other objects muddies important distinctions. Free databse queries can breach the encapsulation of domain objects and aggregates. Exposure of technical infrastructure and database access mechanisms complicates the client and obscures the model driven design.

Therefore:
For each type of object that needs global access, create an object that provide the illusion of an in-momory collection of all obejcts of that type. Set up access through a well-known global interface. Provide repositories only for aggregate roots that actually need direct access. Keep the client focused on the model, delegating all object storage and access to the repositories.

The advantages include:

- They present clients with a simple model for obtaining persistent objects and managing their life cycle.
- They decouple application and domain design from persistence technology, multiple database strategies, or even multiple data sources.
- They communicate design decisions about object access.
- They allow easy substitution of a dummy implementation, for use in testing(typically using an in-memory collection).

## Chapter 7: using the language

- understand the model language
- introducing the applications
  - The application classes are coordinators. They should not work out the answers to the questions they ask. That is the domain layer's job.
- identifying entities and value objects
  - entities: globally uniquely identified by single ID or a combination of attributes; requires continuity of identity; not interchangable with same attributes;
  - value objects: two objects with the same value can be the same one; usually shouldn't reference their owners.
- design associations
  - avoid bidirectional associations; they are ticky to maintain
- Aggregate boundaries
  - The root is a single, specific Entity contained in the Aggregate. The root is the only member of the Aggregate that outside objects are allowed to hold references to.
  - Entities within aggregate boundary are internal to aggregate. It has meaning and identity only in association with aggregate root.
- selecting repositories
  - repositories are prohibited from interior of aggregate
  - start with application requirements to find where requires repository
- pause for refactoring
  - design smell: transaction could fail due to contention, maybe need to reconsider aggregate boundaries
