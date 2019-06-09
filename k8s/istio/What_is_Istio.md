Komutlar;
istio aktif mi? `kubectl get namespace -L istio-injection`

#### What is a service mesh?
mikroservislerin birbirleriyle konusmasi

Its requirements can include; 
* discovery
* load balancing
* failure recovery
* metrics
* monitoring

more complex operational requirements; 
* A/B testing
* canary rollouts
* rate limiting
* access control
* end-to-end authentication.

#### Why use Istio?

u add Istio support to services by deploying a special sidecar proxy
throughout your environment that intercepts all network communication between
microservices, then configure and manage Istio using its control plane
functionality, which includes:

* Automatic load balancing for 
  HTTP, 
  gRPC, 
  WebSocket, 
  TCP traffic.
* Fine-grained control of traffic behavior with rich 
  routing rules, 
  retries, 
  failovers, 
  fault injection.
* A pluggable policy layer and configuration API supporting 
  access controls, 
  rate limits 
  quotas.
* Automatic metrics, 
  logs, 
  traces for all traffic within a cluster, including cluster ingress and egress.
* Secure service-to-service communication in a cluster with 
  strong identity-based authentication and authorization.

#### Core features
* Traffic management
`circuit breaker` servisler arasi yinelenen hatalar olursa, baglantiyi baska
servisler arasinda yeniden olusturma.
* Security
* Observability
Istio’s Mixer component is responsible for policy controls and telemetry
collection
* Platform support
* Integration and customization

#### Architecture
logically split into a ~data plane~ and a ~control plane~.

* the ~data plane~; sidecar, mixer, envoy telemetry buada
* the ~control plane~; management ve configuration burada.

#### Envoy
* Dynamic service discovery
* Load balancing
* TLS termination
* HTTP/2 and gRPC proxies
* Circuit breakers
* Health checks
* Staged rollouts with %-based traffic split
* Fault injection
* Rich metrics

#### Mixer

* attribute process
Mixer is in essence an attribute processing machine. The Envoy sidecar invokes
Mixer for every request, giving Mixer a set of attributes that describe the
request and the environment around the request. Based on its configuration and
the specific set of attributes it was given, Mixer generates calls to a
variety of infrastructure backends.
The primary attribute producer in Istio is Envoy

#### Pilot
The core component used for traffic management in Istio is Pilot, which
manages and configures all the Envoy proxy instances deployed in a particular
Istio service mesh.
Pilot enables service discovery, dynamic updates to load balancing pools and routing tables.



 
#### kaynaklar 
* [what is istio](https://istio.io/docs/concepts/what-is-istio/)
* [istio-on-gke](https://cloud.google.com/istio/docs/istio-on-gke/)
* [Bookinfo Application](https://istio.io/docs/examples/bookinfo/)
* [Examples](https://istio.io/docs/examples/)
* [Tasks](https://istio.io/docs/tasks/)
* [upstream connect error or disconnect/reset before headers. #4999](https://github.com/istio/istio/issues/4999)


