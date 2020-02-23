Site Reliability Engineering

# Introduction

By design, it is crucial that SRE teams are focused on engineering. Without constant engineering, operations load increases and teams will need more people just to keep pace with the workload. Eventually, a traditional ops-focused group scales linearly with service size: if the products supported by the service succeed, the operational load will grow with traffic. That means hiring more people to do the same tasks over and over again.

this means shifting some of the operations burden back to the development team, or adding staff to the team without assigning that team additional operational responsibilities.

their potentially unorthodox approaches to service management require strong management support. For example, the decision to stop releases for the remainder of the quarter once an error budget is depleted might not be embraced by a product development team unless mandated by their management.

SRE as a specific implementation of DevOps.

## Ensure a durable focus on engineering

Google caps operational work for SREs at 50% of their time. Their remaining time should be spent using their coding skills on project work. In practice, this is accomplished by monitoring the amount of operational work being done by SREs, and redirecting excess operational work to the product development teams: reassigning bugs and tickets to development managers, [re]integrating developers into on-call pager rotations, and so on. The redirection ends when the operational load drops back to 50% or lower. This also provides an effective feedback mechanism, guiding developers to build systems that don’t need manual intervention.

## Pursing maximum change velocity without violating a service's SLO

How to determine **error budget**:

- What level of availability will the users be happy with, given how they use the product?
- What alternatives are available to users who are dissatisfied with the product’s availability?
- What happens to users’ usage of the product at different availability levels?

An outage is no longer a "bad" thing—it is an expected part of the process of innovation, and an occurrence that both development and SRE teams manage rather than fear.

## Monitoring

Monitoring should never require a human to interpret any part of the alerting domain. Instead, software should do the interpreting, and humans should be notified only when they need to take action.

There are 3 kinds of valida monitoring output:

Alerts: signify that a human nees to take action immediately in response to something that is either happening or about to happen, in order to improve the situation.

Tickets: signify a human needs to take action, but not immediately. The system cannot automatically handle the situation, but if a human takes action in a few days, no damage will result.

Logging: No one needs to look at this information, but it is recorded for diagnostic or forensic purposes.

## Emergency Response

The most relevant metric in evaluating the effectiveness of emergency response is how quickly the response team can bring the system back to health—that is the mean time to repair(MTTR).

Google SRE relies on on-call playbooks, in addition to exercises such as the "Wheel of Misfortune,"7 to prepare engineers to react to on-call events.

## Change Management:

Best practices in this domain use automation to accomplish the following:

- Implementing progressive rollouts
- Quickly and accurately detecting problems
- Rolling back changes safely when problems arise

## Demand Forecasting and Capacity Planning

Demand forecasting and capacity planning can be viewed as ensuring that there is sufficient capacity and redundancy to serve projected future demand with the required availability.

## Provisioning, Efficiency and Performnace

Provisioning combines both change management and capacity planning.

SREs provision to meet a capacity target at a specific response speed, and thus are keenly interested in a service’s performance. SREs and product developers will (and should) monitor and modify a service to improve its performance, thus adding capacity and improving efficiency.

References:

1. [Google SRE books](https://landing.google.com/sre/books/)
2. [Keys to SRE](https://www.youtube.com/watch?v=n4Wf14e2jxQ)
