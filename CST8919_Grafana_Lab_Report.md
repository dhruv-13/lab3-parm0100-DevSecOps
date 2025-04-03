
# 📊 CST8919 – DevOps: Security and Compliance  
## **Lab Assignment**: Grafana Installation and Dashboard Creation for Ubuntu Server Performance  
**Student Name**: *Dhruv Parmar* 

---

## 🔍 Objective  
The goal of this lab is to install Grafana on an Ubuntu server, set up Azure Monitor as a data source using managed identity authentication, and build a performance dashboard to visualize key system metrics such as CPU usage, memory, and network I/O.

---

## 🖥️ Environment & Prerequisites  

- **Ubuntu Server**: Version 18.04 LTS  
- **Azure Account**: With contributor access to create/manage resources  
- **Grafana Version**: Latest stable  
- **Network Access**: Port 3000 open to access Grafana  
- **Browser**: Google Chrome

---

## ✅ Lab Tasks and Implementation Steps

### 🔧 Task 1: Prepare the Ubuntu Server  

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

✅ *Successfully updated and upgraded all packages.*

---

### 📥 Task 2: Install Grafana  

1. **Install required packages and create keyring directory**  
```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
```

2. **Import Grafana GPG key and add APT repository**  
```bash
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

3. **Install Grafana**  
```bash
sudo apt-get update
sudo apt-get install grafana
```

4. **Start and enable Grafana server**  
```bash
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

5. **Verify Grafana status**  
```bash
sudo systemctl status grafana-server
```

✅ *Grafana is active and running.*

---

### 🌐 Task 3: Connect Grafana to Azure Monitor  

1. **Enable Managed Identity on Azure VM**  
   - Access Azure Portal → VM → Identity → System assigned → Set Status to “On”

2. **Assign Roles**  
   - **Monitoring Reader** on **Azure Monitor**  
   - **Reader** role on the **Subscription**

3. **Edit `grafana.ini` for Managed Identity support**  
   File path: `/etc/grafana/grafana.ini`  
   Add/update the following under `[azure]`:
   ```ini
   [azure]
   managed_identity_enabled = true
   ```

4. **Restart Grafana**  
```bash
sudo systemctl stop grafana-server
sudo systemctl start grafana-server
```

5. **Access Grafana UI**  
   URL: `http://<your-server-ip>:3000`  
   Login: `admin / admin` → Reset password

6. **Configure Azure Monitor Data Source**  
   - Navigate to: **Configuration → Data Sources → Add Data Source**  
   - Select: **Azure Monitor**  
   - Choose: **Authentication via Managed Identity**  
   - Save & Test: ✅ *Connection successful*

---

### 📊 Task 4: Create a Dashboard in Grafana  

1. Go to: **+ → Dashboard → Add new panel**  
2. Select **Azure Monitor** as the data source  
3. Metrics added:
   - CPU Usage
   - Memory % Used
   - Network In/Out Bytes  
4. Panel Customizations:
   - Added thresholds for CPU usage (Green: <50%, Orange: <80%, Red: >80%)
   - Customized panel titles and legends  
5. Dashboard saved as: **Ubuntu Server Performance**

✅ *Dashboard displays real-time data correctly.*

---


## 📝 Reflections & Issues Encountered

- **Issue 1**: Azure Monitor failed to authenticate initially due to missing role assignment.  
  **Solution**: Ensured VM had Monitoring Reader role and restarted Grafana after updating `grafana.ini`.

- **Issue 2**: Port 3000 was blocked on the VM.  
  **Solution**: Opened port 3000 via Azure NSG inbound rule.

- Overall, the lab was successful and gave a hands-on experience of integrating **Grafana with Azure Monitor** using secure **Managed Identity**.


