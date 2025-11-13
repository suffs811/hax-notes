# Prometheus
### Resources
- [In-Depth Guide to Prometheus Server for Penetration Testers and C-Suite Executives - Krishna Gupta](https://krishnag.ceo/blog/in-depth-guide-to-prometheus-server-for-penetration-testers-and-c-suite-executives/)
- [prometheus | Prometheus CLI](https://prometheus.io/docs/prometheus/latest/command-line/prometheus/)
### [Prometheus Architecture](https://prometheus.io/docs/introduction/overview/#components)
The Prometheus ecosystem consists of multiple components, many of which are optional:

- the main [Prometheus server](https://github.com/prometheus/prometheus) which scrapes and stores time series data
- [client libraries](https://prometheus.io/docs/instrumenting/clientlibs/) for instrumenting application code
- a [push gateway](https://github.com/prometheus/pushgateway) for supporting short-lived jobs
- special-purpose [exporters](https://prometheus.io/docs/instrumenting/exporters/) for services like HAProxy, StatsD, Graphite, etc.
- an [alertmanager](https://github.com/prometheus/alertmanager) to handle alerts
- various support tools

Most Prometheus components are written in Go.

---
### Common Attack Vectors
- **API Exploitation**: The Prometheus API provides a wealth of data. Testers should attempt to exploit any misconfigurations, such as unauthenticated access or overly permissive API permissions.
- **Exporter Exploitation**: Many organisations deploy third-party exporters to collect metrics from various systems. Testers should ensure that these exporters are secure, properly configured, and free from vulnerabilities.
- **Server and Endpoint Vulnerabilities**: Prometheus servers should not be exposed to the public internet without proper firewalls, access controls, and encryption. Testers should assess the server’s configuration to ensure that it’s not vulnerable to DoS attacks or unauthorized access.
### Tools and Techniques for Penetration Testing Prometheus
Several tools and techniques can be used to evaluate the security of Prometheus deployments:

- **Burp Suite**: Burp Suite can be used to identify vulnerabilities in Prometheus’ web interface and API.
- **Nmap**: identify exposed Prometheus servers and associated ports.
- **Metasploit**: exploit known vulnerabilities in exporters or Prometheus itself.
- **OWASP ZAP**: used for identifying security issues in Prometheus’ web interfaces.
### Mitigation Strategies for Vulnerabilities
To reduce the risks associated with Prometheus deployments, organisations should adopt several best practices:

- Implement **access control** and **authentication mechanisms** for both Prometheus and its exporters.
- Secure the Prometheus **API endpoints** with encryption (TLS/SSL) and authentication.
- Use **network segmentation** to limit access to Prometheus servers and exporters, ensuring that only authorised personnel or services can access them.
- Ensure that **exporters** are configured securely, and regularly audit them for vulnerabilities.

---
# Management API
Prometheus provides a set of management APIs to facilitate automation and integration.

The current stable HTTP API is reachable under `/api/v1` on a Prometheus server.

Querying Prometheus API: https://ac-programming.com/content/Prom/API/Querying.html
```
GET /-/healthy
HEAD /-/healthy
```
This endpoint always returns 200 and should be used to check Prometheus health.
```
GET /-/ready
HEAD /-/ready
```
This endpoint returns 200 when Prometheus is ready to serve traffic (i.e. respond to queries).
```
PUT  /-/reload
POST /-/reload
```
This endpoint triggers a reload of the Prometheus configuration and rule files. It's disabled by default and can be enabled via the `--web.enable-lifecycle` flag.

Alternatively, a configuration reload can be triggered by sending a `SIGHUP` to the Prometheus process.
```
PUT  /-/quit
POST /-/quit
```
This endpoint triggers a graceful shutdown of Prometheus. It's disabled by default and can be enabled via the `--web.enable-lifecycle` flag.

Alternatively, a graceful shutdown can be triggered by sending a `SIGTERM` to the Prometheus process.