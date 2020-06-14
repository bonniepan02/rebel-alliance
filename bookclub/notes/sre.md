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

# Service Level Objects

## SLI:

An SLI is a service level indicator—a carefully defined quantitative measure of some aspect of the level of service that is provided, such as request latency, error rate, system throughput, and availability.

## SLO

An SLO is a service level objective: a target value or range of values for a service level that is measured by an SLI. A natural structure for SLOs is thus SLI ≤ target, or lower bound ≤ SLI ≤ upper bound. Higher QPS often leads to larger latencies, and it’s common for services to have a performance cliff beyond some load threshold.

Choosing and publishing SLOs to users sets expectations about how a service will perform. This strategy can reduce unfounded complaints to service owners about, for example, the service being slow. Without an explicit SLO, users often develop their own beliefs about desired performance, which may be unrelated to the beliefs held by the people designing and operating the service.

> The solution to this Chubby scenario is interesting: SRE makes sure that global Chubby meets, but does not significantly exceed, its service level objective. In any given quarter, if a true failure has not dropped availability below the target, a controlled outage will be synthesized by intentionally taking down the system. In this way, we are able to flush out unreasonable dependencies on Chubby shortly after they are added. Doing so forces service owners to reckon with the reality of distributed systems sooner rather than later.

## SLA

SLAs are service level agreements: an explicit or implicit contract with your users that includes consequences of meeting (or missing) the SLOs they contain. An easy way to tell the difference between an SLO and an SLA is to ask "what happens if the SLOs aren’t met?": if there is no explicit consequence, then you are almost certainly looking at an SLO.

An understanding of what your users want from the system will inform the judicious selection of a few indicators. Choosing too many indicators makes it hard to pay the right level of attention to the indicators that matter, while choosing too few may leave significant behaviors of your system unexamined.

## Waht do you and your users care about?

- User-facing serving systems: availability, latency, and throughput
- Storage systems: latency, availability, and durability, data integrity
- Big data systems, throughput and end-to-end latency. How much data is being processed? How long does it take the data to progress from ingestion to completion?
- All systems care about correctness.

monitoring metrics: distributions or averages. Monitoring and alerting based only on the average latency would show no change in behavior over the course of the day, when there are in fact significant changes in the tail latency.

A high-order percentile, such as the 99th or 99.9th, shows you a plausible worst-case value, while using the 50th percentile (also known as the median) emphasizes the typical case. And allow an error budget—a rate at which the SLOs can be missed—and track that on a daily or weekly basis.

## Choosing targets

Choosing targets (SLOs) is not a purely technical activity because of the product and business implications, which should be reflected in both the SLIs and SLOs (and maybe SLAs) that are selected.

While understanding the merits and limits of a system is essential, adopting values without reflection may lock you into supporting a system that requires heroic efforts to meet its targets, and that cannot be improved without significant redesign.

SLOs can—and should—be a major driver in prioritizing work for SREs and product developers, because they reflect what users care about. A good SLO is a helpful, legitimate forcing function for a development team. But a poorly thought-out SLO can result in wasted work if a team uses heroic efforts to meet an overly aggressive SLO, or a bad product if the SLO is too lax. SLOs are a massive lever: use them wisely.

## SLOs Set Expectations

### Keep a safety margin

An SLO buffer also makes it possible to accommodate reimplementations that trade performance for other attributes, such as cost or ease of maintenance, without having to disappoint users.

### Don't overachieve

Understanding how well a system is meeting its expectations helps decide whether to invest in making the system faster, more available, and more resilient. Alternatively, if the service is doing fine, perhaps staff time should be spent on other priorities, such as paying off technical debt, adding new features, or introducing other products.

# Eliminating Toil

> If a human operator needs to touch your system during normal operations, you have a bug. The definition of normal changes as your system grow.

## Toil defined

What is toil? Toil is the kind of work tied to running a production service that tends to be manual, repetitive, automatable, tactical, devoid of enduring value, and that scales linearly as a service grows.

Toils is not simply equivalent to administrative chores(overhead, meetings, etc) or grungy work(clean up alerts). If human judgment is essential for the task, there’s a good chance it’s not toil.

Reasons why toil is bad: career stagnation, low morale, slow progress, promote atrrition.  
If we all commit to eliminate a bit of toil each week with some good engineering.

# Monitoring Distributed Systems



References:

1. [Google SRE books](https://landing.google.com/sre/books/)
2. [Keys to SRE](https://www.youtube.com/watch?v=n4Wf14e2jxQ)
