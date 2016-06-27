# Monitoring is Dead: Long Live Monitoring

## Abstract

> Monitoring systems have not changed significantly in 20 years and has fallen
> behind the way be build software. Our software is now large distributed
> systems made up of many non-uniform interacting components while the core
> functionality of monitoring systems has stagnated.  Furthermore, it is often
> people without expert knowledge of systems under observation that are
> responsibile for monitoring and operating them. In this talk, we will explore
> how our current monitoring capabilities are failing us and discuss how we can
> build systems that are both reliable and observable while making our lives
> (or the lives of the people responsible for their operations in production)
> easier.

## References

1. Fischer, M. [Impossibility of Distributed Concensus with One Faulty Process](https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf). in Journal of the Association for Computing Machinery, Vol. 32, No. 2, April 1985, pp. 374-382. 
2. Lamport, L., Shostak, R., and Pease, M. [The Byzantine Generals Problem](http://research.microsoft.com/en-us/um/people/lamport/pubs/byz.pdf). in ACM Transactions on Programming Languages and Systems, Vol. 4, No. 3, July 1982, Pages 382-401.
3. Poledna, S., Burns, A., Wellings, A., and Barrett, P. [Replica Determinism and Flexible Scheduling in Hard Real-Time Dependable Systems](https://people.cs.pitt.edu/~melhem/courses/3530/papers/ft5.pdf). in IEEE Transactions on Computers, Vol. 49, No. 2, February 2000, Pages 100-111.
4. Videla, A. [Failure Modes in Distributed Systems](http://videlalvaro.github.io/2013/12/failure-modes-in-distributed-systems.html). in his blog, December 2013.


## Further Reading

* [A Brief Tour of FLP Impossibility](http://the-paper-trail.org/blog/a-brief-tour-of-flp-impossibility/)
* [Fault Management in Distributed Systems](http://repository.upenn.edu/cgi/viewcontent.cgi?article=1960&context=cis_reports)
* [The Case for Byzantine Fault Detection](https://www.usenix.org/legacy/event/hotdep06/tech/prelim_papers/haeberlen/haeberlen_html/index.html)
* [Fail-Stop Processors](https://www.cs.cornell.edu/fbs/publications/FailStop.pdf)
* [The Phi Accrual Failure Detector](http://fubica.lsd.ufcg.edu.br/hp/cursos/cfsc/papers/hayashibara04theaccrual.pdf)
* [GEMS: Gossip-Enabled Monitoring Service for Scalable Heterogeneous Distributed Systems](http://docs.hcs.ufl.edu/pubs/GEMS2005.pdf)
* [Implementing Fault-Tolerant Services Using the State Machine Approach: A Tutorial](https://www.cs.cornell.edu/fbs/publications/SMSurvey.pdf)
* [A Fault Detection Service for Wide Area Distributed Computations](http://toolkit.globus.org/ftppub/globus/papers/hbm.pdf)
* [Fault Detection and Identification in Computer Networks: A Soft Computing Approach](https://uwspace.uwaterloo.ca/bitstream/handle/10012/4905/NFMS_PhD_Thesis_Jan7.pdf)
* [BAR Fault Tolerance for Cooperative Services](https://www.cs.utexas.edu/~lorenzo/papers/sosp05.pdf)
* [PeerReview: Practical Accountability for Distributed Systems](http://www.sosp2007.org/papers/sosp118-haeberlen.pdf)
* [The Verification of a Distributed System](http://queue.acm.org/detail.cfm?id=2889274)
* [Practical Byzantine Fault Tolerance and Proactive Recovery](http://research.microsoft.com/en-us/um/people/mcastro/publications/p398-castro-bft-tocs.pdf)
* [Accrual Failure Detectors](http://www.jaist.ac.jp/jinzai/Report16/Hayashibara.pdf)
* [Consistency in a Partitioned Network: A Survey](http://repository.upenn.edu/cgi/viewcontent.cgi?article=1669&context=cis_reports)
* [A Gossip-Style Failure Detection Service](https://www.cs.cornell.edu/home/rvr/papers/GossipFD.pdf)
* [SWIM: Scalable Weakly-Consistent Infection-style Process Group Membership Protocol](https://www.cs.cornell.edu/~asdas/research/dsn02-swim.pdf)
* [Adaptive Diagnosis in Distributed Systems](http://www.research.ibm.com/people/r/rish/papers/IEEE.pdf)
* [Dempster-Shafer theory](https://en.wikipedia.org/wiki/Dempster%E2%80%93Shafer_theory)
* [A Simpley View of the Dempster-Shafer Theory of Evidence and its Implication for the Rule of Combination](http://people.eecs.berkeley.edu/~zadeh/papers/Dempster-Shafer_1986.pdf)
* [Beyond Breakpoints: A Tour of Dynamic Analysis](https://github.com/dijkstracula/QConNYC2016)
* [Trust by Verify: Accountability for Network Services](http://issg.cs.duke.edu/publications/trust-ew04.pdf)

## Tools

* [Hystrix](https://github.com/Netflix/Hystrix)
* [Hystrix Dashboard](https://github.com/Netflix/Hystrix/tree/master/hystrix-dashboard)
* [hystrix-go](https://github.com/afex/hystrix-go)
* [go-kit](https://github.com/go-kit/kit)
* [hashicorp/memberlist](https://github.com/hashicorp/memberlist)
