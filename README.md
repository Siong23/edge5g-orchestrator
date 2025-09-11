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
```
+--------------------------------------------------------------------------------+
|                      OSS / BSS / Self-Service Portal (UI / APIs)              |
|         (Service Ordering, Customer Management, Monitoring & Analytics)       |
+-----------------------------------------↑--------------------------------------+
                                          |
               TM Forum Open APIs (Service Catalog, Ordering, Inventory, etc.)   
                                          ↓
+--------------------------------------------------------------------------------+
|                    OpenSlice (Service & Slice Orchestrator)                   |
| - Manages lifecycle of services and network slices                            |
| - Exposes TM Forum APIs                                                       |
| - Handles service catalog, orders, and templates                              |
| - Translates service orders into resource-level orchestration requests        |
+-----------------------------------------↓--------------------------------------+
                             SOL005 / ETSI NFV Orchestration APIs
                                          ↓
+--------------------------------------------------------------------------------+
|           OSM (Open Source MANO - ETSI NFV Orchestrator / NFVO)               |
| - Instantiates, scales, and terminates VNFs and CNFs                          |
| - Interacts with underlying infrastructure via VIMs and CIMs                  |
| - Composed of: RO (Resource Orchestrator), VCA (Juju), MON                    |
+-----------------------------↓-----------------------------↓-------------------+
                              |                             |
              +---------------+---------------+   +---------+---------------+
              |         OpenStack VIM         |   |       Kubernetes CIM     |
              |         (Core / Cloud)        |   |       (Edge / Fog)       |
              +---------------+---------------+   +---------+---------------+
                              |                             |
                +-------------+-----------+     +-----------+-------------+
                |     VNFs (e.g.,         |     |    CNFs (e.g.,          |
                |     MQTT Core,          |     |    Edge Broker,         |
                |     IoT Gateway)        |     |    InfluxDB, AI Model)  |
                +-------------------------+     +-------------------------+
```


## 📚 Documentation
- System Overview
- Deployment Guide
- TMF API Integration
- Slice Templates
- Troubleshooting

