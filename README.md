# Site Reliability Engineering

## How Google Runs Production Systems

---

### Chapter 1

SREs are engineers. They apply the priciples of computer science and engineering to the design and development of large distributed systems.

Tasks can include writing code, developing add ons, managing back ups or load balancing and applying existing solutions to new problems.

SREs are focused on finding ways to improve the design and operation
of systems to make them more scalable, more reliable, and more efficient.

The difference between 99.99% and 100% uptime is not worth the effort and costs involved.

Developers want to quickly release new features while engineers don't want new releases to cause downtime.

By design, it is crucial that SRE teams are focused on engineering.

In general, an SRE team is responsible for
the availability, latency, performance, efficiency, change management, monitoring,
emergency response, and capacity planning of their service(s).

Postmortems that did not trigger a page are even more valuable, as
they likely point to clear monitoring gaps.

That permitted 0.01% unavailability is the service’s error budget. We can spend the budget on anything we want, as long as we don’t overspend it.

So how do we want to spend the error budget? The development team wants to
launch features and attract new users. Ideally, we would spend all of our error budget
taking risks with things we launch in order to launch them quickly.

Monitoring software should do the interpreting, and humans should be notified only when they need to take action.

Valid monitoring outputs are Alerts, Tickets and Logging.

Most outages are in production so we should implement progressive rollouts, quickly and accurately detect problems and roll back safely when problems arise.

Provising the right resources and checking for slowness. A slow or inefficient app equates to a loss of capacity.

### Chapter 2

Tens of machines are placed in a rack
Racks stand in a row
One or more rows form a cluster
Usually a datacenter building houses multiple clusters
Multiple datacenter buildings that are located close together form a campus

Monitoring can be used to: 
Set up alerting for acute problems
Compare behaviour - did the latest update improve performance
Examine how resource consumption evolves over time, capacity planning

Protocol buffers have many advantages over XML for serializing structured data. They're simpler to use, 3-10 times smaller and 20-100 times faster and less ambiguous

### Chapter 3

SRE seeks to balance the risk of unavailability with the goals of rapid innovation and efficient service operations, so that users' overall happiness - with features, service and performance - is optimized. 

Cost dimensions 
- The cost of redundant machine / compute resources: redundant equipment cost
- The opportunity cost: allocating resources to build systems that diminish risk

We strive to make a service reliable enough, but not more than it needs to be.

Risk is based on unplanned downtime.

Time based availability equation 
Availability = uptime / (uptime + downtime)

Request based availability equation
Availability = successful requests / total requests

Factors to consider for risk tolerance
1. What level of availability is required
2. Do different types of failures have different effects on the service
3. How can we use the service cost to help locate a service on the risk continuum
4. What other service metrics are important to take into account

Factors to consider for target availability 
1. What level of service will the users expect
2. Does this service tie directly to revenue (or the users' revenue)
3. Is this a free or paid service
4. Competitors availability metrics
5. Is this service targeted at consumers or at enterprises

Typical Tensions between Developers and SRE
* Software Fault Tolerance
- How hardened do we make the software to unexpected events? Too little = brittle and unstable. Too hardened = no one wants to use it
- Testing. Too little = outages, data leaks, press worthy events. Too much = could lose your market
- Push Frequency. How much should we work on reducing that risk, versus doing other work?
- Canary duration and size: Test a release on small subset of typical workload. How long do we wait and how big is the canary?

Product Management defines an SLO which sets the expectation of uptime. As long as current uptime is above SLO, new releases can be pushed.

Error budget provides a common incentive for both product and SRE to find the right balance between innovation and reliability. 

### Chapter 4

SLI - Service level indicators
- A carefully defined quantitative measure of some aspect of the level of service that is provided. 
- Latency, Error rate (of requests), System throughput (requests per second)
SLO - Service level objectives
- Target or value or range of values for a service level that is measured by an SLI. 
SLA - Service level agreement
- Explicit or implicit contract with your users that includes consquences of meeting (or missing) the SLOs they contain. Explicit consquences unlike SLO's

Yeild = well formed requests that succeed. 

Indicators 
- Too many makes it hard to pay the right level of attention to each indicator
- Too few and you might have large parts of the system un-examined. 

User facing systems - availability, latency and throughput
Storage systems - durability, latency, availability
Big data systems - Throughput, end to end latency
All systems - correctness 

User studies have shown that people typically prefer a slightly slower system to one that has a high variance in repsonse times.

We generally prefer to focus on percentiles rather than average values. This makes it possible to consider long tail of data points which have significantly different (more interesting) characteristics than the average.

Standardize indicators and build templates for them. Measure what your users care about, not what is easy to measure. 

Track SLOs on a daily or weekly basis to identify trends 

Choosing SLO targets
- Don't pick a target based on current performance
- Keep it simple
- Avoid absolutes
- Have as few SLOs as possible
- Perfection can wait

SLOs can - and should - be a major driver in prioritizing work for SREs and product developers, because they reflect what users care about. 

Control Measures
1. Monitor and measure the system's SLIs
2. Compare the SLIs and SLOs and decide whether or not action is needed
3. If action is needed, figure out what needs to happen in order to meet the target
4. Take that action

### Chapter 5

If a human operator needs to touch your system during normal operations, you have a bug. 

Toil = operational work, admin work or grungy work. 

Admin chores are not always toil, sometimes they're overhead. Team meetings, goals, HR work, etc.

Toil elements = Manual, repetitive, automatable, tactical, no enduring value, scales with service growth. 

Typical SRE activities 
1. Software engineering - writing or modifying code. 