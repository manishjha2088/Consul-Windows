# Consul-Windows
Run Consul as a service on Windows 

# 🧭 HashiCorp Consul on Windows (Agent/Server Mode)

This guide helps you install and run [HashiCorp Consul](https://developer.hashicorp.com/consul) on **Windows 10/11** using **NSSM** as a Windows service.

Supports:
- Local agent/server mode
- Web UI on `localhost:8500`
- Auto-start on boot



## 🧰 Prerequisites

- Windows 10 / 11 (x64)
- [Consul for Windows](https://developer.hashicorp.com/consul/downloads)
- [NSSM](https://nssm.cc/download)

---

## 📁 Directory Structure

C:
└── consul
├── consul.exe
├── config
│ └── consul.hcl
└── data



## 🚀 Step-by-Step Setup

### 1️⃣ Download and Extract

- Download Consul ZIP for Windows from:  
  👉 https://developer.hashicorp.com/consul/downloads

- Extract `consul.exe` to:  
  `C:\consul\`

- Download and extract NSSM to:  
  `C:\nssm\`

---

### 2️⃣ Create Directories

```powershell
mkdir C:\consul\data
mkdir C:\consul\config

3️⃣ Create Configuration File
Create: C:\consul\config\consul.hcl

datacenter = "dc1"
data_dir   = "C:/consul/data"
log_level  = "INFO"
ui_config {
  enabled = true
}

server = true
bootstrap_expect = 1
bind_addr = "127.0.0.1"
client_addr = "0.0.0.0"
For client agent mode, set server = false and remove bootstrap_expect.

4️⃣ Run Once to Test Manually

C:\consul\consul.exe agent -config-dir="C:\consul\config"
Open http://127.0.0.1:8500/ui to verify UI works.

5️⃣ Install Consul as a Windows Service
Run in Admin Command Prompt:
C:\nssm\nssm.exe install Consul
In the NSSM GUI:

Application Path:
C:\consul\consul.exe
Arguments:
agent -config-dir="C:\consul\config"
Startup Directory:
C:\consul\

Click Install service.

6️⃣ Start the Service
nssm start Consul
Or use services.msc and set to Automatic startup.

7️⃣ Access the Web UI
Visit:
http://127.0.0.1:8500/ui

🔄 Manage the Service
To stop or restart:
nssm stop Consul
nssm start Consul
✅ Example Commands
Store a key-value pair:

consul kv put hello world
consul kv get hello
Check members:


consul members
⚠️ Production Tips
Bind to a private network interface, not 127.0.0.1

Use proper ACLs and TLS in production

For clusters, set bootstrap_expect = 3 and configure LAN gossip

Use DNS forwarding if integrating with service discovery

📂 About
This repo is maintained by @manishjha2088 for experimenting with HashiCorp Consul on Windows using native tools and service integration.

