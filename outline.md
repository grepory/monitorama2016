1. The tyranny of NRPE: An history lesson
	1. The big five
		1. Resource utilization
			- CPU
			- Memory
			- Disk
		2. Aliveness
			- Ping the machine (literally `ping`)
			- Ping the process (`ps | grep`)
	2. NRPE (Nagios Remote Plugin Executor)
		1. Static thresholds for all!
			- OK: something < something else!
				- This isn't always ok! (0 < 50).
			- WARN: something < something else < another thing!
				- something may be less than something else, but it can be > than it REALLY FAST.
				- Is this _really_ a warning???
				- tornado warning: highly unpredictable and ususally too late
			- CRITICAL: something > something else!
				- OH GOD IT'S PROBABLY ON FIRE.
				- Or maybe it's not! Remember our old friend CPU?
		2. Binary states for everything else!!!
			- It's alive!
				- Yay?
			- It's not alive!
				- OH GOD WHEN DID IT DIE?
    3. We've just been getting _really_ _really_ good at this
      - We have the ability to track these things over time, and in some cases group things together before alerting
      - We've learned how to apply some basic math to our measurements to decide if we should alert or not
      - We're still measure a lot of the same things and then doing a lot of the same sorts of basic assertions about those things, though...
2. Everything you know is wrong
  1. Vocabulary Problems
    1. "Monitoring" and "Observability"
      - Conflating concepts in the space.
			- Competition between vendors (who make very different things) makes this worse.
			- Monitoring and observability are two sides of the same token.
    2. Observability
      - Observability is the ability to observe something.
      - Observability provides visibility into what a system is doing at any given time.
      - Observability may imply metrics, but it doesn't end there.
      - Things are observable and those observations generate stuff that can also be observed.
    3. Monitoring
      - Taking action based on observations.
      - Monitoring just means that you're watching something--sort of like meta-observation.
      - You generally apply rules to the things you observe, and then take action based on those rules.
  2. Monitoring the wrong things or monitoring the right things poorly
    - We monitor on things that aren't operational requirements all the time
    - Why does CPU utilization have to be below 90% at all times?
    - Why does disk use have to be below 80% at all times?
    - Why can't a database have 100 connections to it?
    - Why can't a queue ever get backed up?
    - Alerting on these things only serves to drive your operations staff insane.
    - Much of our monitoring and visibility is ad hoc and ex post facto -- a thing breaks and then we monitor it.
  3. Very bad blackbox monitoring
    - This is usually an organizational problem, but...
    - In our organizations, we have come to at least hope, but hopefully expect developers to inform operations teams about the functional and operational requirements of software. Companies are getting much better at this (thanks, DevOps movement!).
    - In the absence of comprehension of how a system component should operate, the very best you can do is: "Is it running?"
    - If we're expecting this out of internal tooling, why aren't we doing this for the open source software we use, too?
    - Operations teams should not have to divine the operational requirements of your software.
  4. Believing ridiculous nonsense
    - There is only one right way to monitor things
    - You have to keep all of your logs and metrics available all the time forever
    - Logs aren't observability
    - Monitoring isn't metrics
    - Monitoring is only metrics
    - Dashboards are garbage
3. How do we do better?
  1. Fault detection
	  1. New vocabulary
	    - Components
        - individual pieces of a system
        - components have observable properties
        - it is important to understand the operational requirements of components
      - Sensors
        - attached to components to observe their state
        - examples of sensors
          - read/write errors to a dependency over time
          - current heap allocated
          - number of goroutines
      - Agents
        - experts that can evaluate sensor readings
        - codified operational requirements
        - capable of coordinating collections of sensors/components
      - Systems
        - collections of components (also components themselves!)
        - these have operational requirements (as collections of components that also have operaitonal requirements)
        - the requirements associated with components are likely not the same as the requirements associated with a system.
    2. Types of faults
      - Repsond incorrectly
        - program error/bug
        - we won't consider these types of faults too carefully for this talk--much broader subject
      - Respond too slowly / fail to respond
      - Verifiably unavailable
		3. Ways to detect faults
      - First we need the ability to know if something is supposed to respond and if it didn't fast enough or at all
      - Next we must track when this happens, specifically over time
  2. Fault diagnosis
    1. Monitoring
      1. Exposing fault detection to third-party systems
        - APIs, heartbeats, etc. these can be used for more than fault tolerance
        - Monitoring systems can then take action, if possible, including: notifying a person, taking corrective action
      2. Alert on the system's behavior, not an individual component's
        - Do not monitor components in isolation. A single component failing is likely not worth alerting over if it's a replicated component.
        - Instead consider the system as a whole. If this is a service with replicated components (5 copies of the same service) consider service level objectives (latency < 50ms for all requests) and notify when you aren't meeting those -- not for a single component, but when enough components have failed to cause the service not to achieve its operational objectives.
    2. Observability
      1. Make everything that matters observable
        - If it's in your production path, it matters, and it should be observable.
        - If it's open source, and you make it observable, work with the maintainers of the project to get that observability component into the product in a way that everyone can use it.
      2. Ways to observe
        - Dashboards: still a thing. It can be helpful if you don't understand the operational characteristics of your application to simply visualize them.
        - Logs: logs are still a thing, too. It can be easy to detect faults if your service logs meaningful error messages. Furthermore, these log messages can help diagnose errors.
        - Share information about dependency states: if you know you can't reach your database, you can expose this information in an easy-to-identify manner that allows your monitoring system to add context to an alert. Consider the following simple example: { "okay": false, "dependencies": { "database": { "okay": false }}}. That kind of declarative failure message is a great place to start diagnosis. If you can add additional context about the identity and manner of the dependency, then you've saved yourself some time during troubleshooting.
  3. Fault tolerance
    1. Requirements
      - Fault detection
      - Backup plans
    2. Circuit breakers
      1. Circuit breakers (sensors that determine if an external component has "failed")
        - failure here is defined by operational requirements:
        - too many errors (network, not application)
        - cannot connect
        - latency is too high
      2. Exposing circuit breaker state
        - Circuit breakers become observable by exposing their state (open, closed) and the sensor readings
        - Hystrix exposes this via an event stream.
        - You can monitor that event stream so that action can be taken when a circuit opens.
        - Other circuit breakers could expose it via an HTTP API, or any way they want.
      3. Ensemble coordination through gossip protocols
        - This is something I haven't seen in production yet, but that I want to see.
        - Circuit breakers could coordinate by gossiping about services
        - In the simplest form, this would be within a single service (where operational requirements of dependencies are all similar)
        - the basic protocol:
          - consider a service that gossips both about the members of the ensemble and the states of components under observation.
          - with each gossip packet, while updating ensemble membership, members could say they suspect a service as being faulty
          - if enough members vote a service as faulty, then the ensemble could choose to consider that service as unavailable on the whole
          - this would be really cool, because it could inform both the circuit breakers for that external service AND our monitoring system (and thus a human)
      3. Backup plans
        1. Promises, promises
          - Promise != promise
          - Suspect vs. Failed - A service may be suspected of failure (or potential failure), and you can take action on that as well.
        2. Promise theory
          - By (sincerely) promising to do something, I will inherently try harder to uphold that promise.
          - Corrolary: If I know I cannot fulfill a promise (or have good cause to suspect), I should not make that promise.
          - Apply backpressure.
        3. Lower-case p promises
          - Always use promises and always promise to do things.
          - The worst backup plan I have ever heard of in Hystrix: infinitely queue requests until the service recovers
          - If you've tripped a circuit breaker on an external dependency, and your only backup plan is to queue a request, you are going to be considered a Known Bad Actor if all you do is continuously queue requests to the failed dependency. Be nice to your neighbors. Let them recover... Try to fulfill less promises if you suspect them of failure (i.e. circuit is still closed, but dependency is suspect).
4. Closing remarks
  - It may seem as though a lot of what I'm advocating requires changes in how we build software. That is absolutely true. I believed this so passionately, that I have changed my career path--leaving operations after many years and moving into software engineering and technical leadership.
  - This is just how I believe we should do things. I am not advocating that this is a universal truth or even that everyone needs this level of sophistication in their distributed systems. However, building systems like this is easier than you would expect, and there is a _significant_ amount of literature out there that not only tells you why this is how you should build things, but tells you how.
  - Even if you only have a single service that is deployed as a monolith and only has a single dependency, you can still be fault tolerant of that single dependency, and you should, because we're all building a distributed system together. We all use each other's products. We should all behave as though people use and depend on the things that we build.
  - I think more and more, that we, as people who are in or come from operations, will have the ability to influence the way that software is built and contribute to its construction ourselves. Thanks again, DevOps movement. As much as I may dislike the Google SRE book (for many and varied reasons, some petty and some not), it does serve a purpose in forwarding the case for embedded operations within software engineering teams. In order to participate in those teams, however, you must be educated in how to build reliable systems. I hope this helps.