Using Istio’s traffic management model essentially decouples traffic flow and
infrastructure scaling, letting you specify via Pilot what rules they want
traffic to follow rather than which specific pods/VMs should receive traffic

Pilot is responsible for the lifecycle of Envoy instances deployed across the
Istio service mesh.

As shown in the figure above, Pilot maintains a canonical representation of
services in the mesh that is independent of the underlying platform.
Platform-specific adapters in Pilot are responsible for populating this
canonical model appropriately. For example, the Kubernetes adapter in Pilot
implements the necessary controllers to watch the Kubernetes API server for
changes to the pod registration information, ingress resources, and
third-party resources that store traffic management rules. This data is
translated into the canonical representation. An Envoy-specific configuration
is then generated based on the canonical representation.

Pilot consumes information from the service registry and provides a
platform-independent service discovery interface. Envoy instances in the mesh
perform service discovery and dynamically update their load balancing pools
accordingly.

#### Rule configuration
* `VirtualService` defines the rules that control how request for a service
  are routed within an istio service mesh (ism)
* A `DestinationRule` configures the set of policies to be applied to a
  request after `VirtualService` routing has occured
* A `ServiceEntry` is commonly used to enable requests to services outside of
  an ism
* A `Gateway` configures a load balancer operating at the edge of the mesh for
  http/tcp ingress trafic to a mesh application or egress traffice to external
services.
* A `Sidecar` configures one or more sidecar proxies attached to appliction
  workloads running inside the mesh
