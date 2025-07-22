# edge5g-orchestrator 📶⚙️

> **A cloud-native orchestration framework for managing 5G network services at the edge.**  
> Automate, scale, and secure your distributed telco workloads with zero-touch orchestration and full lifecycle management.

---

## 🌐 Overview

`edge5g-orchestrator` is an open platform that enables zero-touch deployment, management, and monitoring of 5G core and edge network functions (VNFs/CNFs). The system integrates TM Forum Open APIs, ETSI NFV standards, and Kubernetes-native tools to create a fully programmable service orchestration pipeline for telecom and IoT use cases.

---

## 🚀 Key Features

- 🧠 **Zero-Touch Orchestration** of 5G services and slices from customer order to resource allocation
- 🔌 **Open API Integration** via TM Forum (Service Catalog, Service Order, Resource APIs)
- ☁️ **Multi-Domain Support** (Core cloud, Edge cloud, VIM/CIM)
- 📡 **Kubernetes-Native CNF Management** with Helm, CRDs, and service mesh
- 🔐 **Network Slicing with Isolation** using Kubernetes namespaces and policies
- 📊 **Integrated Monitoring & Telemetry** (Prometheus, Grafana, Jaeger)

---

## 🧱 Architecture Diagram

                 +-------------------------------------------+
                 |     OSS / BSS / Portal (TMF Open APIs)    |
                 |  (Ordering, Monitoring, Customer Mgmt)    |
                 +--------------------↑----------------------+
                                      |
                                      | TMF APIs (Order, Catalog)
                                      ↓
      +--------------------------------------------------------------+
      |         OpenSlice (Service & Slice Orchestrator)             |
      |  - Service lifecycle, catalog mgmt, slice template mgmt      |
      |  - Translates TMF API calls to orchestration instructions    |
      +--------------------↓-------------------↓---------------------+
                            |                   |
                            | SOL005 / NFV APIs | Kubernetes APIs
                            ↓                   ↓
      +--------------------------+     +-----------------------------+
      | OSM / NFVO               |     | Kubernetes (Edge / Core)    |
      | - VNF lifecycle mgmt     |     | - CNFs: 5G Core, AI, MQTT   |
      | - RO, VCA, MON           |     | - Namespaces for isolation  |
      +--------------------------+     +-----------------------------+


---

## 📦 Installation

### ✅ Requirements

- Kubernetes v1.24+
- Helm v3.11+
- OpenSlice & TM Forum API Gateway
- (Optional) OSM or other NFVO
- Docker, kubectl

### 🔧 Setup

```
git clone https://github.com/your-org/edge5g-orchestrator.git
cd edge5g-orchestrator
```
Install Helm charts:
```
helm install orchestrator ./helm/orchestrator -n orchestrator --create-namespace
```
Deploy CNFs:
```
helm install cnfs ./helm/cnfs -n edge-core
```

## Project Structure
```
edge5g-orchestrator/
├── helm/                   # Helm charts for orchestrator and CNFs
├── apis/                   # TM Forum API adapters and handlers
├── descriptors/            # NSD/VNFD and slice templates
├── charts/                 # Pre-packaged CNFs (AI, MQTT, 5G core)
├── docs/                   # Documentation and diagrams
├── monitoring/             # Prometheus, Grafana, Jaeger configs
└── README.md
```

## Use Cases
- Deploy and manage 5G core components and edge apps
- Create, scale, and delete network slices on demand
- Integrate vertical IoT applications via CNFs
- Monitor performance and anomalies at the edge

## 📊 Monitoring
- Grafana: http://<host>:3000
- Prometheus: http://<host>:9090
- Jaeger: http://<host>:16686

## 📚 Documentation
- System Overview
- Deployment Guide
- TMF API Integration
- Slice Templates
- Troubleshooting

