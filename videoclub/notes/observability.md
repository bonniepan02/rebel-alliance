# Observability

## Why do we need observability?

It comes down to one primary fact: software is becoming exponentially more complex, both on the infrastructure side and the product side: a convergence of patterns like microservices, polyglot persistence, containers that continue to decompose monoliths into agile, complex systems and an explosion of platforms and creative ways for empowering humans to do cool new stuff. Systems complexity is a mixture of essential, and accidental complexity including complexity from software and infrastructure. Infrastructure complexity comes from types of services, ways of communicating between them, exposure to the external network, storage types, security requirements, etc. The way to combat accidental complexity both in software and infrastructure is to continously refactor and simply the architecture, focusing on locality and simplicity.

When environments are as complex as they are today, simply monitoring for known problems doesn’t address the growing number of new issues that arise. These new issues are “unknown unknowns,” meaning that without an observable system, you don’t know what is causing the problem and you don’t have a standard starting point/graph to find out.
This escalating complexity is why observability is now necessary to build and run today’s infrastructure and services.

## What is observability?

Observability is about being able to ask arbitrary questions about your environment without—and this is the key part—having to know ahead of time what you wanted to ask.

Observability is feedback that provides insight into a process and refers to the work needed to extract meaning from the available data.

A system is “observable” to the extent that you can explain what is happening on the inside just from observing it on the outside without, and this is key, without having to ship new code every time.

Traditional monitoring relies heavily on predicting how a system may fail and checking for those failures. Observability is not a sum of metrics, logging, and distributed Tracing.

In simple terms, ODD is somewhat cyclical, looping through these steps:

1. What do your users care about? Instrument to measure that thing.
2. Now that you are collecting data on how well you do the things your users care about,
   use the data to get better at doing those things.
3. Improve instrumentation to also collect data about user behaviors--how users try
   to use your service--so you can inform feature development decisions and priorities.
   Once you have answered at least the first question in this list, you are ready to make strides toward achieving observability.

Treat owning your code and instrumenting it thoroughly as a first class responsibility, and as a requirement for shipping it to production.

No pull request should ever be accepted without being able to answer, how will I know if this doesn't work? Which is not something that we're used to. Observability-driven development as extention behind TDD means you ship it safely, a small amount to production and you watch it and you see what happens. And you gain confidence. Own your operational execellence, especially for your core differentiators, because the cost and pain of developing software is approximately zero compared to the operational cost of maintaining it overtime.

We believe in Allspaw’s declaration that debugging must forever be a human-centered process. Observability is to make it as pleasant and collaborative a process as possible. Learn from expensive incidents, and build people up: learn and grow and solve meaningful problems.

Telemetry refers to all the data and signals your services produce about themselves. Rather than focusing on the shape of the telemetry - as the three pillars perspective will have you do - focus on what kinds of questions you’re trying to answer and let that guide your choice of telemetry. For example this is how you can utilize your telemetry for: Health, Availability,Debuggability.

## References

1. [Lightstep: three pillars with zero answers: towards a new scorecard for observability](https://lightstep.com/blog/three-pillars-zero-answers-towards-new-scorecard-observability/)
2. [Honeycomb guide: achieving observability](https://www.honeycomb.io/wp-content/uploads/2018/07/Honeycomb-Guide-Achieving-Observability-v1.pdf)
3. [Observability: a 3-year retrospective](https://thenewstack.io/observability-a-3-year-retrospective/)
4. [Monitoring and observability](https://medium.com/@copyconstruct/monitoring-and-observability-8417d1952e1c)
5. [Journey into observability: Telemetry](https://mads-hartmann.com/sre/2020/01/11/journey-into-observability-telemetry.html)
