# Monitoring is Dead: Long Live Monitoring

## Abstract

> While the sophistication of what they can do has changed, the core abilities of monitoring systems have not kept pace with the software they're being used to monitor. The advent of large, distributed systems made up of many non-uniform interacting components has made the job of operating those systems considerably difficult. Furthermore, it is often people without expertise in how the systems operate that are responsibile for their operation. In this talk, we will explore how our current monitoring capabilities are failing us and discuss how we can .

## References

* [GEMS: Gossip-Enabled Monitoring Service for Scalable Heterogeneous Distributed Systems](http://docs.hcs.ufl.edu/pubs/GEMS2005.pdf)
* [Implementing Fault-Tolerant Services Using the State Machine Approach: A Tutorial](https://www.cs.cornell.edu/fbs/publications/SMSurvey.pdf)
* [Fault Management in Distributed Systems](http://repository.upenn.edu/cgi/viewcontent.cgi?article=1960&context=cis_reports)

## Further Reading

Some of these fall into multiple categories. I left them where I thought they belonged at the time I read them.

### Fault detection

* [The Byzantine Generals Problem](http://research.microsoft.com/en-us/um/people/lamport/pubs/byz.pdf)
* [The Case for Byzantine Fault Detection](https://www.usenix.org/legacy/event/hotdep06/tech/prelim_papers/haeberlen/haeberlen_html/index.html)
* [A Fault Detection Service for Wide Area Distributed Computations](http://toolkit.globus.org/ftppub/globus/papers/hbm.pdf)
* [Fault Detection and Identification in Computer Networks: A Soft Computing Approach](https://uwspace.uwaterloo.ca/bitstream/handle/10012/4905/NFMS_PhD_Thesis_Jan7.pdf)
* [The Phi Accrual Failure Detector](http://fubica.lsd.ufcg.edu.br/hp/cursos/cfsc/papers/hayashibara04theaccrual.pdf)
* [BAR Fault Tolerance for Cooperative Services](https://www.cs.utexas.edu/~lorenzo/papers/sosp05.pdf)
* [PeerReview: Practical Accountability for Distributed Systems](http://www.sosp2007.org/papers/sosp118-haeberlen.pdf)
* [The Verification of a Distributed System](http://queue.acm.org/detail.cfm?id=2889274)
* [Practical Byzantine Fault Tolerance and Proactive Recovery](http://research.microsoft.com/en-us/um/people/mcastro/publications/p398-castro-bft-tocs.pdf)
* [Accrual Failure Detectors](http://www.jaist.ac.jp/jinzai/Report16/Hayashibara.pdf)
* [Consistency in a Partitioned Network: A Survey](http://repository.upenn.edu/cgi/viewcontent.cgi?article=1669&context=cis_reports)
* [A Gossip-Style Failure Detection Service](https://www.cs.cornell.edu/home/rvr/papers/GossipFD.pdf)
* [Fail-Stop Processors](https://www.cs.cornell.edu/fbs/publications/FailStop.pdf)
* [SWIM: Scalable Weakly-Consistent Infection-style Process Group Membership Protocol](https://www.cs.cornell.edu/~asdas/research/dsn02-swim.pdf)

### Fault diagnosis

* [Adaptive Diagnosis in Distributed Systems](http://www.research.ibm.com/people/r/rish/papers/IEEE.pdf)
* [Dempster-Shafer theory](https://en.wikipedia.org/wiki/Dempster%E2%80%93Shafer_theory)
* [A Simpley View of the Dempster-Shafer Theory of Evidence and its Implication for the Rule of Combination](http://people.eecs.berkeley.edu/~zadeh/papers/Dempster-Shafer_1986.pdf)

### Evidence generation

* [Beyond Breakpoints: A Tour of Dynamic Analysis](https://github.com/dijkstracula/QConNYC2016)
* [Trust by Verify: Accountability for Network Services](http://issg.cs.duke.edu/publications/trust-ew04.pdf)

## Tools

* [Hystrix](https://github.com/Netflix/Hystrix)
* [Hystrix Dashboard](https://github.com/Netflix/Hystrix/tree/master/hystrix-dashboard)
* [hystrix-go](https://github.com/afex/hystrix-go)
* [go-kit](https://github.com/go-kit/kit)
* [hashicorp/memberlist](https://github.com/hashicorp/memberlist)
