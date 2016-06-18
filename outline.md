1. The tyranny of NRPE: An history lesson
	1. The big five
		1. Resource utilization
			- CPU
			- Memory
			- Disk
		2. Aliveness
			- Ping the machine (literally `ping`)
			- Ping the process (`ps | grep`)
	2. NRPE
		1. Static thresholds for all!
			- OK: something < something else!
				- This isn't always ok! (0 < 50).
			- WARN: something < something else < another thing!
				- something may be less than something else, but it can be > than it REALLY FAST.
				- Is this _really_ a warning???
				- This feels more like a tornado warning--and we know how well those work.
			- CRITICAL: something > something else!
				- OH GOD IT'S PROBABLY ON FIRE.
				- Or maybe it's not! Remember our old friend CPU?
		2. Binary states for everything else!!!
			- It's alive!
				- Yay?
			- It's not alive!
				- OH GOD WHEN DID IT DIE?
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
3. How do we do better?
  1. Fault detection (New vocabulary!)
    - Components: individual pieces of your system
    - Sensors: use these to take "measurements" of the state of components
    - Agents: experts that can evaluate sensor readings
    - Systems: collections of components (also components themselves!)
  2. Fault diagnosis
    1. Monitoring
      1. Exposing fault detection states to monitoring systems
      2. Alert on the system's behavior, not an individual component's
    2. Observability
  3. Evidence generation
    1. There are many very good talks and papers about this, for examples of this see the README.md
4. Fault tolerance
  1. Requirements
    - Fault detection
    - Backup plans
  2. Fault detection
    1. Hystrix (Circuit Breakers)
      1. Circuit breakers (sensors that determine if an external component has "failed")
        - failure here is defined by operational requirements:
        - too many errors
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
        - There are papers about this in my README.
    2. Backup plans
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
