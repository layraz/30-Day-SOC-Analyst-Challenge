# Day 3: Elasticsearch Setup

## Objective

Set up Elasticsearch on a cloud instance (Vultr) as the core storage/indexing tier for our SOC SIEM lab. This machine will later be integrated with Kibana, Logstash/Beats, and other logging sources.

## 1. Cloud environment setup on Vultr

Provider: [https://www.vultr.com](https://www.vultr.com)

### Create a VPC network

- Go to Network → VPC 2.0.
- Click Create VPC.
- Name: MyDFIR-SOC-Challenge.
- Configure the subnet (for example, 172.31.0.0/24) and save.

### Deploy the Elasticsearch VM

- Go to Compute → Instances → Deploy a Server.

#### Step 1: Select Location and Plan

- Type: Dedicated CPU
- Location: Tokyo, Japan
- Plan: General Purpose (voc-g-4c-16gb-80s)
- Disable Automatic Backup

#### Step 2: Configure Software and Deploy Instance

- OS: Ubuntu 22.04 LTS x86_64
- VPC Network: MyDFIR-SOC-Challenge
- Server Hostname: MyDFIR-ELK
- Instance Connectivity: Public IPv4

After deployment, Vultr provisions the instance with a public IP (for example, PUBLIC_IP).

1. Access the instance via SSH
    
    From your local machine (Windows example):
    
    ```bash
    C:\Users\SOC> ssh root@PUBLIC_IP
    ```
    

On first connect, you will see the host key prompt:

```bash
The authenticity of host 'PUBLIC_IP (PUBLIC_IP)' can't be established.
ED25519 key fingerprint is ...
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

Then enter the password provided by Vultr to log in.

Once inside, update the system:

```bash
root@MyDFIR-ELK:~# apt update && apt upgrade -y
```

## 2. Install Elasticsearch

### Download the Debian package

Download Elasticsearch 9.3.4 (amd64) from the Elastic artifacts repository:

```bash
root@MyDFIR-ELK:~# wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-9.3.4-amd64.deb
```

### Install the package

```bash
root@MyDFIR-ELK:~# sudo dpkg -i elasticsearch-9.3.4-amd64.deb
```

During installation, Elastic will generate a password for the built‑in elastic superuser.

Save this password securely (for example, in a password manager or temporary note).

## 3. Configure `elasticsearch.yml`

Edit the main Elasticsearch configuration file:

```bash
root@MyDFIR-ELK:~# cd /etc/elasticsearch
root@MyDFIR-ELK:/etc/elasticsearch# nano elasticsearch.yml
```

### Allow remote access from your SOC analyst machine

To allow your local laptop (on the VPC subnet) to communicate with Elasticsearch, add:

```bash
network.host: PUBLIC_IP
```

This binds Elasticsearch to the instance’s public IP so Kibana/Beats can reach it.

### Ensure HTTP port is enabled

If not already uncommented, enable the HTTP port:

```bash
http.port: 9200
```

Save and close the file.

## 4. Restrict external access with a firewall

To prevent public internet access to MyDFIR-ELK while still allowing access from your SOC‑VPC subnet:

- Go to Vultr: Settings → Firewall → Manage → Create Firewall Group.
- Name: soc-internal-only.

#### Add an IPv4 Rule:

- Action: Allow
- Protocol: TCP
- Port: 9200
- Source: Custom – 172.31.0.0/24

Then apply the firewall group to the instance:

- Go to Compute → MyDFIR-ELK → Settings → Firewall.
- Select the firewall group soc-internal-only.

Now only machines inside the VPC network (172.31.0.0/24) can reach Elasticsearch on port 9200.

## Configure Elasticsearch as a systemd service

Enable and start Elasticsearch as a system service:

```bash
root@MyDFIR-ELK:~# sudo systemctl daemon-reload
root@MyDFIR-ELK:~# sudo systemctl enable elasticsearch.service
root@MyDFIR-ELK:~# sudo systemctl start elasticsearch.service
```

Check the status:

```bash
root@MyDFIR-ELK:~# systemctl status elasticsearch.service
```

Example expected output (shortened):

```bash
● elasticsearch.service - Elasticsearch
     Loaded: loaded (/lib/systemd/system/elasticsearch.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2026-05-04 05:10:56 UTC; ...
       Docs: https://www.elastic.co
   Main PID: 26135 (java)
      Tasks: 104
     Memory: 8.3G
        CPU: 54.215s
```

If the service is active (running), Elasticsearch is up and listening on http://PUBLIC_IP:9200.
