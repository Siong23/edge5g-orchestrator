The figure below shows a UML sequence diagram for the demo workflow of the edge5g-orchestrator project. It illustrates how a user initiates a 5G service deployment, and how the system handles orchestration, provisioning, monitoring, and feedback.

![your-UML-diagram-name](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/Siong23/edge5g-orchestrator/refs/heads/main/demo/workflow.puml)

### Description of Workflow:
1. User submits a service order via a self-service portal.
2. The portal forwards the order using TM Forum APIs to the orchestrator.
3. The orchestrator handles service decomposition and determines which CNFs/VNFs need to be deployed.
4. The orchestrator interacts with the Kubernetes cluster using Helm charts or CRDs to deploy and configure CNFs.
5. Once deployed, the network functions (like Open5GS, MQTT brokers, etc.) become active and start operation.
6. Monitoring tools like Prometheus and Grafana continuously collect telemetry from the CNFs.
7. This data is sent back to the orchestrator and portal, providing zero-touch feedback to the user.
