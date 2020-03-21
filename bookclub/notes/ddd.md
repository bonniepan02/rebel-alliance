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
