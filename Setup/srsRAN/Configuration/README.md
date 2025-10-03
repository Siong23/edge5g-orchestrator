## 3️⃣ srsRAN Configuration on Server 1
### 📝 3.1 Edit Open5gs gNB File
#### 3.1.1 Check in directory `srsRAN_Project/config/your_gnb_file.yaml`
```yaml
# This example configuration outlines how to configure the srsRAN Project gNB to create a single FDD cell
# transmitting in band 3, with 20 MHz bandwidth and 30 kHz sub-carrier-spacing. A USRP N3XX is configured
# as the RF frontend using split 8. Note in this example the internal GPDSO of the N310 is used.

cu_cp:
  amf:
    addr: 192.168.0.195
#    addr: 192.168.0.177
    port: 30412
    bind_addr: 192.168.0.195
#    bind_addr: 192.168.0.195
    supported_tracking_areas:
      - tac: 7
        plmn_list:
          - plmn: "00101"
            tai_slice_support_list:
              - sst: 1
                sd: 0x1

ru_sdr:
  device_driver: uhd
  device_args: type=x300

  clock: internal
  sync: internal
  srate: 23.04
  tx_gain: 20
  rx_gain: 20

cell_cfg:
  dl_arfcn: 650000
  band: 77
  channel_bandwidth_MHz: 20
  common_scs: 30
  plmn: "00101"
  tac: 7
  pci: 1


log:
  filename: /tmp/gnb.log
  all_level: info

pcap:
  mac_enable: false
  mac_filename: /tmp/gnb_mac.pcap
  ngap_enable: false
  ngap_filename: /tmp/gnb_ngap.pcap
```
⚠️ **Note:**  
When running the gNB, make sure the following values in `open5gs_gnb.yaml` are the same as your **Open5GS configuration**:  

- **AMF IP address (`amf.addr`)** → must match the AMF service IP in Open5GS  
- **Port (`amf.port`)** → default is `38412` in Open5GS, use same in gNB config  
- **PLMN (`plmn`)** → must match the MCC/MNC configured in Open5GS (e.g., `"00101"`)  
- **Tracking Area Code (`tac`)** → must match Open5GS `tac`  
- **Slice (`sst`, `sd`)** → must match the slice defined in Open5GS  

✅ If these values don’t match, the gNB will fail to establish NGAP connection with Open5GS.

&nbsp;

### 🗄️ 3.2 Check Subsribers information in MongoDB
#### 3.2.1 Terminal to MongoDB
```bash
kubectl exec -it -n <your_namespace> <mongodb_pod_name> -- mongosh open5gs
```
#### 3.2.2 List Subscribers
```bash
db.subscribers.find().pretty()
```
⚠️ **Note:** Ensure IMSI, key, and OPC match the UE config

---

## ✅ 4. Run Components
### 📡 4.1 Open one terminals
- 🖥️ Terminal 1: Start gNB
```bash
sudo gnb -c <your_gNB_filename>
```
