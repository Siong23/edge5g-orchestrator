The figure below shows a UML sequence diagram for the demo workflow of the edge5g-orchestrator project. It illustrates how a user initiates a 5G service deployment, and how the system handles orchestration, provisioning, monitoring, and feedback.

![your-UML-diagram-name](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/Siong23/edge5g-orchestrator/refs/heads/main/demo/workflow.puml)

### Description of Workflow:
1. User Interaction:
    - A user (e.g., network admin, service operator) logs into the Self-Service Portal and submits a service request to deploy a 5G network slice or application.
2. API-Driven Service Order:
    - The portal communicates this service order to the Service Orchestrator using TM Forum Open APIs (such as Service Order, Service Catalog, and Resource APIs).
3. Service Orchestration:
    - The Orchestrator processes the service request, validates it, and decomposes it into resource-level requirements.
    - Based on service requirements (e.g., latency, locality, resource type), the orchestrator selects the appropriate infrastructure:
      - OpenStack (for VNFs that require VM-based deployment, like EPC core functions).
      - Kubernetes (for containerized CNFs, like MQTT brokers or AI inference services).
4. Interaction with OSM (Open Source MANO):
    - The orchestrator triggers OSM to instantiate the required Network Service Descriptors (NSDs) and Virtual Network Function Descriptors (VNFDs).
    - OSM acts as a multi-VIM orchestrator and translates the descriptors into deployment actions.
5. Infrastructure Deployment:
    - For VNFs, OSM interacts with OpenStack, provisioning virtual machines and configuring networking.
    - For CNFs, OSM interfaces with Kubernetes (via Helm charts or CRDs) to deploy microservices in the edge cloud.
6. Service Activation:
    - The 5G service components (like Open5GS, srsRAN, AI CNFs, etc.) are launched and begin operating across the hybrid infrastructure.
7. Monitoring and Observability:
    - All deployed functions emit telemetry (logs, metrics, traces) to the Monitoring Stack, which includes:
      - Prometheus for metrics collection,
      - Grafana for visualization,
      - Jaeger for distributed tracing.
8. Feedback Loop:
    - The monitoring stack reports health and performance data to the Orchestrator.
    - The orchestrator can take this data into account for scaling, recovery, or adaptation (in case of policy-driven or closed-loop control extensions).
9. User Feedback:
   - The Portal is updated in real-time with service deployment status, KPIs, and system health dashboards for end-users to monitor.
10. Demo Completion:
    - The user can interactively observe service instantiation, view network and service health, and verify zero-touch orchestration from end to end.
