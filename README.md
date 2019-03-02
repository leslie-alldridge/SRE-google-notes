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
