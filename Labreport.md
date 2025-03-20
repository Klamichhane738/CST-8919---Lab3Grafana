  
# CST 8919 Lab Assignment: Grafana Installation and Dashboard Creation for Ubuntu Server Performance  

### Student Name: Keshav Lamichhane  
### Student ID:  041170863
### Date:  20th March 2025

---

## Introduction  
Grafana is an open-source platform for monitoring and observability. It allows users to visualize and analyze metrics collected from various data sources, including Azure Monitor. This lab focuses on setting up Grafana on an Ubuntu server, integrating it with Azure Monitor, and creating a performance monitoring dashboard.

## Objective  
The objective of this lab is to install Grafana on an Ubuntu machine, configure Azure Monitor as a data source, and create a Grafana dashboard to visualize system performance metrics such as CPU usage, memory usage, and network I/O.

## Prerequisites  
- An Ubuntu server (version 18.04 or later)  
- An Azure account with permissions to create and manage Azure resources  
- Basic understanding of the Linux command line interface  

---

## Steps  

### Deploying VM in azure to host grafana.
![alt text](<Screenshots/Screenshot (256).png>)

### Step 1: Prepare Your Ubuntu Server  
```bash
sudo apt-get update && sudo apt-get upgrade
```
![alt text](<Screenshots/Screenshot (265).png>)

### Step 2: Install Grafana  
#### Add Grafana's official APT repository and install Grafana:  
```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana
```


#### Start and enable the Grafana server:  
```bash
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```
![alt text](<Screenshots/Screenshot (272).png>)
#### Verify the service is running:  
```bash
sudo systemctl status grafana-server
```
![alt text](<Screenshots/Screenshot (317).png>)

#### Enable Port 3000 (if necessary):  
```bash
sudo ufw allow 3000/tcp
```
![alt text](<Screenshots/Screenshot (293).png>)

#### Screenshot of Grafana running:

![alt text](<Screenshots/Screenshot (274).png>)
---

### Step 3: Connect Grafana to Azure Monitor  
#### Enable Managed Identity in your Grafana VM.  

![alt text](<Screenshots/Screenshot (260).png>)
#### Assign Monitoring Reader Role on Azure Monitor and Reader Role on your Subscription. 

#### Assigning Monitoring Reader Role
![alt text](<Screenshots/Screenshot (285).png>)

#### Assigning Reader Role to Subscription
![alt text](<Screenshots/Screenshot (280).png>)

#### Enable managed identity authentication for data sources:  
Edit the Grafana configuration file:

Run the following command to receive 'edit permission' to this file: grafana.ini

![alt text](<Screenshots/Screenshot (294).png>)
Set the required authentication parameters.

![alt text](<Screenshots/Screenshot 2025-03-20 143445.png>)

#### Restart the Grafana server:  
```bash
sudo systemctl stop grafana-server
sudo systemctl start grafana-server
```
![alt text](<Screenshots/Screenshot (316).png>)
#### Open Grafana in your web browser:  
Access it via `http://<your-server-ip>:3000`  
Default credentials: `admin/admin`

![alt text](<Screenshots/Screenshot (275).png>)

#### Configure the Data Source:  
1. Navigate to **Configuration > Data Sources**
2. Click on **Add data source**
3. Select **Azure Monitor**
4. Choose **Managed Identity** for authentication
5. Save and test the connection

![alt text](<Screenshots/Screenshot (297).png>)

Here I encountered an error saying "No Log Analytics Workspace Found" so I created one.
![alt text](<Screenshots/Screenshot (299).png>)

---
Finally connection established.

![alt text](<Screenshots/Screenshot (301).png>)

### Step 4: Create a Dashboard in Grafana  
1. Click on the **“+”** icon and select **Dashboard**
2. Click **Add new panel**
3. Select **Azure Monitor** as the data source
4. Choose metrics such as **CPU usage, Memory usage, Network I/O**
5. Customize with thresholds, colors, and labels
6. Save the panel and the dashboard

![alt text](<Screenshots/Screenshot (302).png>)
 
![alt text](<Screenshots/Screenshot (304).png>)

Here select resources metrics to visualize.
![alt text](<Screenshots/Screenshot (309).png>)
**To visualize it better i created a dashboard from template**

![alt text](<Screenshots/Screenshot (321).png>)



#### Screenshot of the Grafana Dashboard:
 ### Last 15 min visualization

![alt text](<Screenshots/Screenshot (319).png>)

 ### Last 1 hour visualization
 ![alt text](<Screenshots/Screenshot (320).png>)

## Issues Encountered & Resolutions  
## Issues Encountered & Resolutions  

### 1. Unable to Edit `grafana.ini` Config File  
**Issue:**  
I was unable to edit the `grafana.ini` config file to set the managed identity to `true`.  

**Resolution:**  
Ran the following command to change the file ownership:  
sudo chown $USER:$USER /etc/grafana/grafana.ini
![alt text](<Screenshots/Screenshot (294).png>)

### 2. Health Check Failed When Connecting to Azure Monitor  
**Issue:**  
One or more health checks failed when connecting Azure Monitor using managed identity and subscription. The error indicated that no Log Analytics workspace was found.  

![alt text](<Screenshots/Screenshot (298).png>)

**Resolution:**  
Manually created a Log Analytics workspace, which resolved the issue.

## Conclusion  
This lab demonstrated the installation and configuration of Grafana on an Ubuntu server, the integration of Azure Monitor for performance data collection, and the creation of a monitoring dashboard. By following these steps, users can effectively monitor their server's performance using Grafana's visualization capabilities.


