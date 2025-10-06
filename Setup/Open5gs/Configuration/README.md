## 6️⃣ Access Open5GS WebUI
🌐 Step 1: Check Service Name
```bash
kubectl get svc -n <namespace>
```
> 💡 Replace `<namespace>` with your actual namespace.

&nbsp;

🔁 Step 2: Port Forward WebUI
```bash
kubectl port-forward svc/<open5gs-webui-svc-name> 9999:9999 -n <namespace>
```
> 💡 Replace `<open5gs-webui-svc-name>` and `<namespace>` with your actual svc-name and namespace.

&nbsp;

🖥️ Step 3: Open in Browser

Once the port forwarding is active, open your browser and navigate to:
👉 [http://localhost:9999](http://localhost:9999)

Log in using the default credentials:
- **Username:** `admin`
- **Password:** `1423`

Once logged in, you can add or edit subscribers in the WebUI.

🎉 Open5GS installation via OSM and Kubernetes is complete!
