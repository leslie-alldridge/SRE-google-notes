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
1. Software engineering - writing or modifying code. Adding service features for scalability and reliability
2. Systems engineering - Configuring production systems, documenting systems, load balancers, architecture, etc
3. Toil - Work directly tied to running a service that is repetitive, manual, etc
4. Overhead - Admin work not tied to running a service directly. Hiring, paperwork, HR, admin, etc

Aim is to invent more and toil less. 

### Chapter 6

Monitoring - Collecting, processing, aggregating, and displaying real-time quantitative data about a system, such as query counts and types, error counts and types, processing times and server lifetimes. 

White-box monitoring - metrics based on exposed internals of the system

Black-box monitoring - testing external behaviour as a user would see it

Dashboard - Summary view of a service's core metrics. 

Alert - Notification intended to be read by a human and pushed to a system (queue)

Root cause - A defect in a software or human system that, if repaired, instills confidence that this event won't happen again in the same way.

Node and machine - single instance of running kernel (virtual machine, container, physical server)

Push - Any change to a service's running software or its configuration

Why Monitor? 
* Analyzing long-term trends 
* Comparing over time or experiment groups
* Alerting 
* Building Dashboards 
* Conducting ad hoc retrospective analysis (debugging)

Your monitoring system should address two questions: what's broken, and why?

The Four Golden Signals
1. Latency - the time it takes to service a request
2. Traffic - A measure of how much demand is being placed on your system
3. Errors - The rate of requests that fail 
4. Saturation - How full your service is or is going to be

Take care in how the measurements are structured. Collecting CPU logs every second is expensive to collect, store and analyze. 

Guidelines to keep in mind 
1/ The rules that catch real incidents most often should be simple, predictable and reliable. 
2/ Data collection that is rarely used should be up for removal
3/ Signals that are collected but not displayed anywhere should be up for removal

Monitoring principles 
* Does this rule detect an otherwise undetected condition that is urgent, actionable, and actively or imminently user-visible?
* Will I ever be able to ignore this alert, and how can I avoid this scenario
* Does the alert definitely indicate users are being negatively affected
* Can I take action in response to this alert, is it a long/short term fix
* Are other people getting paged for this issue, rendering a page unnecessary

Pages should be an event we haven't seen before, require intelligence, actionable and able to be responded to urgently. 

A healthy monitoring and alerting pipeline is simple and easy to reason about and support long term plans.

### Chapter 7

If automation runs regularly and successfully enough, the result is a reduced mean time to repair for those comon faults. 

The later in a product lifecycle a problem is discovered, the more expensive it is to fix. 

If we aren't creating processes and solutions that can be automated, we continue having to staff resources to maintain those solutions.

Automation is meta-software, software to act on software.

Examples of where automation can be used: User account creation, cluster turn up and turn down, software or hardware preparation and decommissioning, config changes, and many more.

Automation processes can vary in three respects
1. Competence (their accuracy)
2. Latency (how quickly all steps are executed)
3. Relevance, or proportion of real-world process covered by automation. 

Automation provides more than just time savings

Standard good practices in software engineering will help considerably: having decoupled subsystems, introducing APIs, minimizing side effects, and so on. 

### Chapter 8

SREs focus on deployable code with a reliable release process. Release management covers release velocity, consistent and repeatable builds, product team self service and enforcement of policies and procedures.

It's best to invest in a solid release pipeline at the outset as it's more expensive to tack it on as an afterthought. 

### Chapter 9

At the end of the day, our job is to keep agility and stability in balance in the system.

SRE teams should:
- Push back when accidental complexity is introduced into systems they're responsible for
- Constantly strive to eliminate complexity in systems they assume operational responsibility for

Removing dead code is better than commenting it out or flagging it as no good

Software simplicity is a prerequisite to reliability.

### Chapter 10

Monitoring is a very large system due to:
- the sheer number of components being analyzed
- the need to maintain a reasonbly low maintenance burden on the engineers responsbile for the system

White box monitoring does not provide a full picture of the system being monitored; you aren't aware of what users are seeing.

Treating time-series data as a data source for generating alerts. 

### Chapter 11

When on-call the engineer is available to perform operations on production systems within minutes. The engineer is expected to triage the problem and work toward its resolution, involving others and escalating if necessary. 

Most teams will have a primary and secondary on call engineer. 

Multi-site teams are best to avoid night shifts being a requirement. 

When dealing with outages related to complex systems, being rational, focused and deliberate is best. 

Most important on-call resources are: 
- Clear escalation paths
- Well-defined incident-management procedures
- A blameless postmortem culture

Escalating when a serious outage has significant unknown dimensions

Alerts to event ratio should be 1:1 to allow engineers time to focus on the problem instead of triaging alerts.

In some cases SRE will give the pager back to the Product Team - until it reaches a state where SRE can look after it. 

### Chapter 12 

Troubleshooting process: 
- Triage (Problem reported)
- Examine
- Diagnose
- Test / Treat (Cure)

Assessing an issues severity requires an exercise of good engineering judgement and, often, a degree of calm under pressure. 

First course of action should be to make the system as well as it can under the circumstances. E.g. diverting traffic, spinning up new instances, etc.

Examine - using timeseries data and metrics. Logs are also helpful. 

Diagnose - send test data to applications, work your way through the stack, ask "what, where, why". "What touched it last, what changed recently". 

Consider the obvious first - it's better to ping an application before diving into its code base. 

Often we can only find probable (not actual) cause because systems are complex and reproducing in a live production system may not be an option. Having a non production environment is helpful for this reason, but costly to run. 

