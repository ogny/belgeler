An Operator is an application-specific controller that extends the Kubernetes
API to create, configure, and manage instances of complex stateful
applications on behalf of a Kubernetes user. 

challenge is managing stateful applications, like databases, caches, and
monitoring systems. These systems require application domain knowledge to
correctly scale, upgrade, and reconfigure while protecting against data loss
or unavailability. We want this application-specific operational knowledge
encoded into software that leverages the powerful Kubernetes abstractions to
run and manage the application correctly.

An Operator is software that encodes this domain knowledge and extends the
Kubernetes API through the third party resources mechanism, enabling users to
create, configure, and manage applications. Like Kubernetes's built-in
resources, an Operator doesn't manage just a single instance of the
application, but multiple instances across the cluster.











#### CustomResourceDefinitions





















