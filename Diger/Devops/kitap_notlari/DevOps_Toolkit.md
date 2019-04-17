git hook'la su su kodu run et

yazarin fikri:
Ansible proved that orchestration does not need to be complicated to set up nor
hard to maintain. With the appearance of Docker, containers are slowly
replacing VMs as the preferable way to create immutable deployments.

Continuous integration (CI) usually refers to integrating, building, and
testing code within the development environment. It requires developers to
integrate code into a shared repository often.
The continuous integration pipeline should run on every commit or push. Unlike
continuous delivery, continuous integration does not have a clearly defined
goal of that pipeline. Saying that one application integrates with others does
not tell us a lot about its production readiness. 

#### CI flow 

*  Pushing to the code repository
*  Static analysis
is the analysis of computer software that is performed without actually
executing programs. The static analysis goals vary from highlighting possible
coding errors to making sure that agreed formatting is followed.  (kodu
çalıştırmadan, sadece formatın düzgün olup olmadığını test ediyor.)
*  Pre-deployment testing
Pre-deployment testing is probably the most critical phase in continuous
integration pipeline. While it does not provide all the certainty that we need,
and it does not substitute post-deployment testing, tests run in this phase are
relatively easy to write, should be very fast to execute and they tend to
provide much bigger code coverage than other types of tests (for example
integration and performance). (anlayamadim)
*  Packaging and deployment to the test environment
step is to create a container that contains not only the package but also all
other dependencies our application might need (libraries, runtime, application
server, and so on). Once deployment package is created, we can proceed to
deploy it to a test environment. 
*  Post-deployment testing
functional, integration and performance tests.
use behavior-driven development for all functional tests that, at the same
time, act as acceptance criteria and Gatling¹¹ for performance tests.

#### CD ve Deployment flow 

CD süreçleri CI'ninkiyle aynı. Farkı, işlemlerin sonucunun doğrudan prod'a yansıması.
We need to pay particular attention to databases (especially when they are
relational) and ensure that changes we are making from one release to another
are backward compatible and can work with both releases (at least for some
time).

we need to run tests that prove that the deployed software is integrated with
the rest of the system. The fact that we run, possibly same, integration tests
in other environments does not mean that due to some differences, software
deployed to production continues to “play nicely” with the rest of the system.

Mikroservisler
The rise of microservices has been a remarkable advancement in application
development and deployment. With microservices, an application is developed,
or refactored, into separate services that “speak” to one another in a well-defined way –
via APIs, for instance. Each microservice is self-contained, each maintains its own
data store (which has significant implications), and each can be updated independently
of others.
Moving to a microservices-based approach makes app development faster and easier
Microservices can help accomplishing less time to deploy to production. Running
the whole pipeline for a huge monolithic application is often slow. Same
applies to testing, packaging, and deployment. On the other hand, microservices
are much faster for the simple reason that they are much smaller. Less code to
test, less code to package and less code to deploy. (mikroservisler tasarımı
parçalara bölerek test ve production'a çıkma sürelerini kısaltıyor.) 

Containers
Mikroservislerin yaygın kullanıldığı zaman, herkes farklı framework'leri
kullanırken, deployment'ta sorunlar oluyordu. bunu ortadan kaldırmak için her
framework'un sabit bir sürümünün kullanmasına yönelindi. bu standardizasyon da
yaratıcılığı ve özgürlüğü engelliyordu. Docker ile bu sorun çözüldü.
uygulamanın bir docker imajı içinde çalışması, uygulamayı platform bağımsız
yapıyor. Bu da production'da bir hatayla karşılaşılma riskini minimuma
indiriyor.
ayrıca lightweight, sistemin kaynaklarını paylaşıyor, ayrı bir os değil.

#### Synergy of Continuous Deployment, Microservices, and Containers

Together, they can combine all that and do so much more. Throughout this book,
we’ll be on a quest to deploy often and fast, be fully automatic, accomplish
zero-downtime, have the ability to rollback, provide constant reliability
across environments, be able to scale effortlessly, and create self-healing
systems able to recuperate from failures. Any of those goals is worth a lot.
Can we accomplish all of them? Yes! Practices and tools we have at our disposal
can provide all that, and we just need to combine them correctly. The journey
ahead is long but exciting. There are a lot of things to cover and explore and
we need to start from the beginning; we’ll discuss the architecture of the
system we are about to start building.


### Sistem Mimarisi
#### Monolitik uygulamalar
uygulamanın bir jar/dll vb. paket hali.
uygulama katmanlara ayrılıyor, olumlu bir gelişme, fakat totalde monolitik
olduğunda nher node'da tüm katmanların bulunması zorunlu. mikroservislerin
doğuşu vm/docker arasındaki bu temel farka benziyor. vm de monolitik yapıda.

#### Service oriented arch.

* Boundaries are explicit
* Services are autonomous
* Services share schema and contract but not class
* Services compatibility is based on policy
* SOA was such a big hit that many software provides jumped right i

We continued having the same layers as we had before, but this time physically
separated from each other. There is an apparent benefit from this approach in
that we can, at least, develop and deploy each layer independently from others.
Another improvement is scaling.

#### Microservices

Microservices are an approach to architecture and development of a single
application composed of small services. The key to understanding microservices
is their independence. Each is developed, tested and deployed separately from
each other. Each service runs as a separate process. The only relation between
different microservices is data exchange accomplished through APIs they are
exposing. They inherit, in a way, the idea of small programs and pipes used in
Unix/Linux. Most Linux programs are small and produce some output. That output
can be passed as input to other programs. When chained, those programs can
perform very complex operations. It is complexity born from a combination of
many simple units.

Microservices movement is, in a way, reaction to misinterpretation of SOA and
the intention to go back to where it all started. The main difference between
SOA and microservices is that the later should be self-sufficient and
deployable independently of each other while SOA tends to be implemented as a
monolith.  

##### Key aspects of microservices are as follows.
* They do one thing or are responsible for one functionality.
* Each microservice can be built by any set of tools or languages since each is
  independent of others.
* They are truly loosely coupled since each microservice is physically
  separated from others.
* Relative independence between different teams developing different
  microservices (assuming that APIs they expose are defined in advance).
* Easier testing and continuous delivery or deployment.


