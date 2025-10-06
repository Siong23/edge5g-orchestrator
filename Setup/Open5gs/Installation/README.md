# 🧩 Open5GS Installation Guide (via OSM + Kubernetes)

This guide explains how to install and configure **Open5GS Core Network** as a Kubernetes Network Function (KNF) using **Open Source MANO (OSM)**.

---

## 1️⃣ Installing Ubuntu 22.04
- 💾 Create a bootable USB with Ubuntu 22.04.
  
  - 💽 **Base Image:** Ubuntu 22.04 (64-bit)  
    - [Ubuntu 22.04 **Cloud Image** (64-bit required)](https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64-disk-kvm.img)
    - [Ubuntu 22.04 **Server Image** (64-bit required)](http://releases.ubuntu.com/22.04/)

- 🖥️ Install on Server 1 and set up user credentials.
  
&nbsp;

⚠️ **Note:**
**OSM is required** to deploy and manage Open5GS as a Kubernetes Network Function (KNF).

If OSM is **not yet installed**, please complete the OSM installation first before continuing.  
👉 [View OSM Installation Guide](../../OSM/Installation/README.md)

> ⚙️ Ensure OSM is running properly and accessible before proceeding with Open5GS installation.

---

## 2️⃣ Add Kubernetes Cluster to OSM
🧠 Step 1: Create a Dummy VIM Target
```bash
osm vim-create \
  --name dummyvim \
  --user admin \
  --password admin \
  --tenant admin \
  --account_type dummy \
  --auth_url http://localhost/dummy
```

⚙️ Step 2: Associate the K8s Cluster
```bash
osm k8scluster-add <cluster_name> \
  --creds ~/.kube/config \
  --vim dummyvim \
  --k8s-nets '{k8s_net1: null}' \
  --version "<k8s_version>" \
  --description "Isolated Kubernetes cluster"
```

---

## 3️⃣ Set Up Core Network: Open5GS
📦 Step 1: Create Persistent Volumes for MongoDB
- Create `open5gs-pv-pvc.yaml`
```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: db-pv
spec:
  storageClassName: manual
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteOnce
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data/vol
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: open5gs-mongodb-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 30Gi

```

- Apply
```bash
kubectl apply -f open5gs-pv-pvc.yaml
```

- Ensure path exists and has the proper file access rights on the host system
```bash
sudo mkdir -p /mnt/data/vol
sudo chown -R 1001:1001 /mnt/data/vol
```

&nbsp;

📥 Step 2: Download Open5GS Helm Chart
```bash
cd ~/osm_packages
mkdir open5gs_knf && cd open5gs_knf
mkdir helm-chart-v3s && cd helm-chart-v3s
helm pull oci://registry-1.docker.io/gradiant/open5gs --version 2.2.5
tar -xvzf open5gs-2.2.5.tgz
```

&nbsp;

🛠️ Step 3: Configure Helm Chart Values
- Edit values.yaml and set parameters
```yaml
dbURI: "mongodb://{{ .Release.Name }}-mongodb/open5gs"

populate:
  enabled: false
  image:
    repository: gradiant/open5gs-dbctl
    tag: 0.10.3
  initCommands:
    - open5gs-dbctl add_ue_with_apn 001010123456789 00112233445566778899aabbccddeeff 63bfa50ee6523365ff14c1f45f88737d srsapn

mongodb:
  enabled: true
  auth:
    enabled: false

amf:
  enabled: true
  config:
    networkName: "srsRAN"
    guamiList:
      - plmn_id:
          mcc: "001"
          mnc: "01"
        amf_id:
          region: 2
          set: 1
    taiList:
      - plmn_id:
          mcc: "001"
          mnc: "01"
        tac: [7]
    plmnList:
      - plmn_id:
          mcc: "001"
          mnc: "01"
        s_nssai:
          - sst: 1
            sd: "0x1"

webui:
  enabled: true
  services:
    http:
      type: NodePort
      ports:
        http: 9999
    nodePorts:
      http: 30002
```

&nbsp;

🧾 Step 4: Create CNF Package
```bash
cd ~/osm_packages/open5gs_knf
nano open5gs_vnfd.yaml
```

- Paste the following
```yaml
vnfd:
  id: open5gs_knf
  product-name: open5gs_knf
  description: Open5GS Core Network deployed as a Kubernetes Network Function via Helm chart
  version: '1.0'
  mgmt-cp: mgmt-ext
  df:
  - id: default-df
  ext-cpd:
  - id: mgmt-ext
    k8s-cluster-net: mgmtnet
  k8s-cluster:
    nets:
    - id: mgmtnet
  kdu:
  - name: open5gs-core
    helm-chart: open5gs
```

- Validate and upload
```bash
cd ..
osm nfpkg-create open5gs_knf
```

---

## 4️⃣ Create Network Service (NS)
📘 Step 1: Create NS Descriptor (NSD)
```bash
cd ~/osm_packages
mkdir open5gs_ns && cd open5gs_ns
nano open5gs_nsd.yaml
```

- Paste the following
```yaml
nsd:
  nsd:
  - id: open5gs_ns
    name: open5gs_ns
    version: '1.0'
    designer: OSM
    description: NS deploying Open5GS Core only
    df:
    - id: default-df
      vnf-profile:
      - id: open5gs
        vnfd-id: open5gs_knf
        virtual-link-connectivity:
        - constituent-cpd-id:
          - constituent-base-element-id: open5gs
            constituent-cpd-id: mgmt-ext
          virtual-link-profile-id: mgmtnet
    virtual-link-desc:
    - id: mgmtnet
      mgmt-network: true
    vnfd-id:
    - open5gs_knf
```

- Validate and upload
```bash
cd ..
osm nspkg-create open5gs_ns
```

---

## 5️⃣ Deploy the Service
🚀 Step 1: Instantiate NS
```bash
osm ns-create --ns_name 5gSA_ns --nsd_name open5gs_ns --vim_account dummyvim
```

⏳ Step 2: Wait for pods to start
```bash
kubectl get pods -A
```
