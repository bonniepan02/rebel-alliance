# Serverless

## What is Serverless?

Serverless is both an architecture and a platform.

Serverless is a cloud computing execution model where the cloud provider dynamically manages the allocation and provisioning of servers. A serverless application runs in stateless compute containers that are event-triggered, ephemeral (may last for one invocation), and fully managed by the cloud provider.

Serverless applications are event-driven cloud-based systems where application development rely solely on a combination of third-party services, client-side logic and cloud-hosted remote procedure calls (Functions as a Service). Cloud providers offers automatic scaling on demand, granular billing for execution time, fault tolerant and highly available.

Thus it enables "Focus on the application, not on the infrastructure". Even though infrastructure is invisible, we still need to own our operational excellence with consideration of observability, triage, maintainability, and think ahead about migration cost.

If we think microservices is to deploy code to systems we build in the cloud with containers and kubernetes. Serverless is another abstraction to deploy code in the cloud.

## Related Tools

### Serverless Framework
Though perhaps poorly named, the [Serverless Framework](https://serverless.com/) offers a suite of tools for packing and deploying serverless applications to several [infrastructure providers](https://serverless.com/framework/docs/providers/). It's installable as an NPM CLI.

The YML configuration syntax is much slimmer and more declarative than something like AWS CloudFormation. There is also a reasonably large plug-in community to add functionality.

Editor's note: The framework assumes a certain app topology, similar to most frameworks. I've used with AWS and it typically does well the more you let it manage. When you start specifying externally managed entities (eg. IAM roles produced by terraform) it becomes slowly more unwieldy. I don't see this as a tool to combat vendor lock-in; it seems impossible to readily switch "provider" configurations without requiring significant additional reconfiguration and code refactoring.

## Other relevant concepts

- Docker
- Kubenetes
- Single Page Application
- Message Broker
- Observability

## References

1. [What is Serverless Architecture? What are its Pros and Cons?](https://hackernoon.com/what-is-serverless-architecture-what-are-its-pros-and-cons-cc4b804022e9)
2. [Serverless Architectures from Mike Roberts](https://martinfowler.com/articles/serverless.html)
3. [Serverless Architectural Patterns](https://medium.com/@eduardoromero/serverless-architectural-patterns-261d8743020)
4. [Own Operational Excellence from Charity Majors](https://charity.wtf/2016/05/31/wtf-is-operations-serverless/)

