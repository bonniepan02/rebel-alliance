Site Reliability Engineering

# Introduction

By design, it is crucial that SRE teams are focused on engineering. Without constant engineering, operations load increases and teams will need more people just to keep pace with the workload. Eventually, a traditional ops-focused group scales linearly with service size: if the products supported by the service succeed, the operational load will grow with traffic. That means hiring more people to do the same tasks over and over again.

this means shifting some of the operations burden back to the development team, or adding staff to the team without assigning that team additional operational responsibilities.

their potentially unorthodox approaches to service management require strong management support. For example, the decision to stop releases for the remainder of the quarter once an error budget is depleted might not be embraced by a product development team unless mandated by their management.

SRE as a specific implementation of DevOps. Site Reliability Engineering represents a significant break from existing industry best practices for managing large, complicated services. Motivated originally by familiarity--"as a software engineer, this is how I would want to invest my time to accomplish a set of repetitive tasks" -- it has becomes

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

# Chapter 9 Simplicity

> The price of reliabiltiy is the pursuit of the utmost simplicity.

If we stop changing the codebase, we stop introducing bugs. (The second law of thermodynamics, in principle, states that a closed system's disorder cannot be reduced, it can only remain unchanged or increase. A measure of this disorder is entropy. This law also seems plausible for software systems; as a system is modified, its disorder, or entropy, tends to increase. This is known as software entropy. Code refactoring is considered to decrease the software entrophy in a stepwise fashion.)

> Goal: at the end of the day, our job is to keep agility and stability in balance in the system.

Exploratory coding: setting an explicit shelf life for whatever code I write with the understanding that I will need to try and fail once in order to really understand the task I need to accomplish.

> SRE works to create procedures, practices and tools that ender software more reliable. Building reliability into development allows developers to focus their attention on what we really do care about—the functionality and performance of their software and systems.

## The virtue of boring

It is very important to consider the difference between essential complexity and accidental complexity.
To minimize accidental complexity, SRE team should:

1. Push back when accidental complexity is introduced into the system for which they are responsible
2. Constantly strive to eliminate complexity in systems they onboard and for which they assume operational responsibility

## I won't give up my code

every new line of code written is a liability. SRE promotes practices that make it more likely that all code has an essential purpose, such as scrutinizing code to make sure that it actually drives business goals, routinely removing dead code, and building bloat detection into all levels of testing.

## The "nagative lines of code" metric

## Minimal APIs

> perfection is finally attained not when there is no longer more to add, but when there is no longer anything to take away

## Modularity

As a system grows more complex, the separation of responsibility between APIs and between binaries becomes increasingly important.

A well-designed distributed system consists of collaborators, each of which has a clear and well-scoped purpose.

## conclusion:

This chapter has repeated one theme over and over: software simplicity is a prerequisite to reliability.
Every time we say "no" to a feature, we are not restricting innovation; we are keeping the environment uncluttered of distractions so that focus remains squarely on innovation, and real engineering can proceed.

References:

1. [Google SRE books](https://landing.google.com/sre/books/)
2. [Keys to SRE](https://www.youtube.com/watch?v=n4Wf14e2jxQ)
