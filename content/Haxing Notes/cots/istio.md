# Istio
Istio extends Kubernetes to establish a programmable, application-aware network.

An Istio service mesh is logically split into a **data plane** and a **control plane**.
- The **data plane** is composed of a set of intelligent proxies ([Envoy](https://www.envoyproxy.io/)) deployed as sidecars. These proxies mediate and control all network communication between microservices. They also collect and report telemetry on all mesh traffic.
- The **control plane** manages and configures the proxies to route traffic.
### Resources
- [Istio / Architecture](https://istio.io/latest/docs/ops/deployment/architecture/)
