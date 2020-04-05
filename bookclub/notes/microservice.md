# Chapter 1

Microservices are an approach to distributed systems that promote the use of finely grained services with their own lifecycles, which collaborate together. Microservices are primarily modeled around business domains. They are small, autonomous services that work together. It is an architecture approach to decompose systems.

DDD helped us understand the importance of representing the real world in our code, and showed us better ways to model our systems. Continuous delivery instills us the idea that we should treat every check-in as a release candidate. Domain-driven design. Continuous delivery. On-demand virtualization. Infrastructure automation. Small autonomous teams. Systems at scale. Microservices have emerged from this world.

Cohesion--the drive to have related code grouped together-- is an important concept when we think about microservices. This is reinforced by Uncle Bob's Single Responsibility Principle.

How small? We seem to have a good sense of what is too big, and so it could be argued that once a piece of code no longer feels too big, it's probably small enough.

As you get smaller(inter-dependence increase), but so too does complexity.

All communication between services themselves are via network calls, to enforce separation between the services and avoid the perils of tight coupling.

The golden rule: can you make a change to a service and deploy it by itself without changing anything else?

To do decoupling well, you will need to model your services right and get the APIs right.

## No silver bullet

Microservices are no free lunch or silver bullet, and make for a bad choice as a golden hammer. They have associated complexities of distributed systems, and don't be surprised if things like distributed transactions or _CAP theorem_ start giving you headaches!

# Chapter 2
