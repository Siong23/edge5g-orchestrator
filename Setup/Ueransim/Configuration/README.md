## 3️⃣ Ueransim Configuration on Server 1
### 📝 3.1 Edit Open5gs gNB File
#### 3.1.1 Check in directory UERANSIM/config/open5gs_gnb.yaml
```yaml
mcc: '001'                # Mobile Country Code (must match Open5GS PLMN)
mnc: '01'                 # Mobile Network Code (must match Open5GS PLMN)

nci: '0x000000010'        # NR Cell Identity
idLength: 32              # gNB ID length in bits
tac: 1                    # Tracking Area Code (must match Open5GS config)

linkIp: 127.0.0.1         # Local IP address of this gNB
ngapIp: 127.0.0.1         # IP for NG interface (to connect with Open5GS AMF)
gtpIp: 127.0.0.1          # IP for GTP-U interface (to connect with UPF)

# List of AMF address information
amfConfigs:
  - address: 127.0.0.1    # AMF IP address
    port: 38412           # AMF NGAP port (default: 38412)

# List of supported S-NSSAIs by this gNB
slices:
  - sst: 1
    sd: '0x1'

# Indicates whether or not SCTP stream number errors should be ignored.
ignoreStreamIds: true
```

&nbsp;

### 📝 3.2 Edit Open5gs UE File
#### 3.2.1 Check in directory UERANSIM/config/open5gs_ue.yaml
```yaml
# IMSI number of the UE. IMSI = [MCC|MNC|MSISDN] (In total 15 digits)
supi: 'imsi-001010987654321'
# Mobile Country Code value of HPLMN
mcc: '001'
# Mobile Network Code value of HPLMN (2 or 3 digits)
mnc: '01'
# SUCI Protection Scheme : 0 for Null-scheme, 1 for Profile A and 2 for Profile B
protectionScheme: 0
# Home Network Public Key for protecting with SUCI Profile A
homeNetworkPublicKey: '5a8d38864820197c3394b92613b20b91633cbd897119273bf8e4a6f4eec0a650'
# Home Network Public Key ID for protecting with SUCI Profile A
homeNetworkPublicKeyId: 1
# Routing Indicator
routingIndicator: '0000'

# Permanent subscription key
key: '465B5CE8B199B49FAA5F0A2EE238A6BC'
# Operator code (OP or OPC) of the UE
op: 'E8ED289DEBA952E4283B54E88E6183CA'
# This value specifies the OP type and it can be either 'OP' or 'OPC'
opType: 'OPC'
# Authentication Management Field (AMF) value
amf: '8000'
# IMEI number of the device. It is used if no SUPI is provided
imei: '356938035643803'
# IMEISV number of the device. It is used if no SUPI and IMEI is provided
imeiSv: '4370816125816151'

# Network mask used for the UE's TUN interface to define the subnet size  
tunNetmask: '255.255.255.0'

# List of gNB IP addresses for Radio Link Simulation
gnbSearchList:
  - 192.168.0.195

# UAC Access Identities Configuration
uacAic:
  mps: false
  mcs: false

# UAC Access Control Class
uacAcc:
  normalClass: 0
  class11: false
  class12: false
  class13: false
  class14: false
  class15: false

# Initial PDU sessions to be established
sessions:
  - type: 'IPv4'
    apn: 'srsapn'
    slice:
      sst: 1
      sd: '0x1'

# Configured NSSAI for this UE by HPLMN
configured-nssai:
  - sst: 1
    sd: '0x1'

# Default Configured NSSAI for this UE
default-nssai:
  - sst: 1
    sd: '0x1'

# Supported integrity algorithms by this UE
integrity:
  IA1: true
  IA2: true
  IA3: true

# Supported encryption algorithms by this UE
ciphering:
  EA1: true
  EA2: true
  EA3: true

# Integrity protection maximum data rate for user plane
integrityMaxRate:
  uplink: 'full'
  downlink: 'full'
```
⚠️ **Note:** The IMSI, key, and OPC values must also exist in your Open5GS database (MongoDB)

&nbsp;

### 🗄️ 3.3 Check Subsribers information in MongoDB
#### 3.3.1 Terminal to MongoDB
```bash
kubectl exec -it -n <your_namespace> <mongodb_pod_name> -- mongosh open5gs
```
#### 3.2.1 List Subscribers
```bash
db.subscribers.find().pretty()
```
⚠️ **Note:** Ensure IMSI, key, and OPC match the UE config

---

## ✅ 4. Run Components
### 📡 4.1 Open three terminals
- 🖥️ Terminal 1: Start gNB
```bash
./build/nr-gnb -c config/open5gs_gnb.yaml
```
- 🖥️ Terminal 2: Start UE
```bash
./build/nr-ue -c config/open5gs_ue.yaml
```
- 🖥️ Terminal 3: Verify UE Interface
```bash
ip addr show uesimtun0
ping -I uesimtun0 8.8.8.8
```
🎉 If successful → UE is registered and using Open5GS for data connectivity.
