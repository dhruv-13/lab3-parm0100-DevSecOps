# lab3-parm0100-DevSecOps

Lab Assignment: Grafana Installation and Dashboard Creation for Ubuntu Server Performance
Student Name: Dhruv Parmar

Objective
The goal of this lab is to install Grafana on an Ubuntu server, set up Azure Monitor as a data source using managed identity authentication, and build a performance dashboard to visualize key system metrics such as CPU usage, memory, and network I/O.

Environment & Prerequisites
Ubuntu Server: Version 18.04 LTS

Azure Account: With contributor access to create/manage resources

Grafana Version: Latest stable

Network Access: Port 3000 open to access Grafana

Browser: Google Chrome

Lab Tasks and Implementation Steps
🔧 Task 1: Prepare the Ubuntu Server
bash
Copy
Edit
sudo apt-get update && sudo apt-get upgrade -y
Successfully updated and upgraded all packages.

Task 2: Install Grafana
Install required packages and create keyring directory

bash
Copy
Edit
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
Import Grafana GPG key and add APT repository

bash
Copy
Edit
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
Install Grafana

bash
Copy
Edit
sudo apt-get update
sudo apt-get install grafana
Start and enable Grafana server

bash
Copy
Edit
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
Verify Grafana status

bash
Copy
Edit
sudo systemctl status grafana-server
Grafana is active and running.

Task 3: Connect Grafana to Azure Monitor
Enable Managed Identity on Azure VM

Access Azure Portal → VM → Identity → System assigned → Set Status to “On”

Assign Roles

Monitoring Reader on Azure Monitor

Reader role on the Subscription

Edit grafana.ini for Managed Identity support
File path: /etc/grafana/grafana.ini
Add/update the following under [azure]:

ini
Copy
Edit
[azure]
managed_identity_enabled = true
Restart Grafana

bash
Copy
Edit
sudo systemctl stop grafana-server
sudo systemctl start grafana-server
Access Grafana UI
URL: http://<your-server-ip>:3000
Login: admin / admin → Reset password

Configure Azure Monitor Data Source

Navigate to: Configuration → Data Sources → Add Data Source

Select: Azure Monitor

Choose: Authentication via Managed Identity

Save & Test: Connection successful

Task 4: Create a Dashboard in Grafana
Go to: + → Dashboard → Add new panel

Select Azure Monitor as the data source

Metrics added:

CPU Usage

Memory % Used

Network In/Out Bytes

Panel Customizations:

Added thresholds for CPU usage (Green: <50%, Orange: <80%, Red: >80%)

Customized panel titles and legends

Dashboard saved as: Ubuntu Server Performance

Dashboard displays real-time data correctly.

## Screenshots

![alt text](image-2.png)

![alt text](image.png)

![alt text](image-1.png)

![alt text](image-4.png)

![alt text](image-5.png)

![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)

![alt text](image-9.png)



