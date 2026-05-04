# Day 4: Kibana Setup

## 1. Download and install Kibana

Download Kibana 9.3.4 `.deb` package:

```bash
wget https://artifacts.elastic.co/downloads/kibana/kibana-9.3.4-amd64.deb
```

Install the package:

```bash
sudo dpkg -i kibana-9.3.4-amd64.deb
```

---

## 2. Configure Kibana

Kibana configuration file:

```bash
sudo nano /etc/kibana/kibana.yml
```

```bash
server.port: 5601
server.host: "<VULTR_PUBLIC_IP>"
```

---

## 3. Start and enable Kibana service

Reload `systemd`and start Kibana:

```bash
sudo systemctl daemon-reload
sudo systemctl start kibana.service
sudo systemctl enable kibana.service
```

Check status:

```bash
systemctl status kibana.service
```

Expected output (shortened):

```bash
● kibana.service - Kibana
     Loaded: loaded (/lib/systemd/system/kibana.service; enabled; vendor preset: enabled)
     Active: active (running) since ...
   Main PID: 27053 (node)
      Tasks: 11
     Memory: 402.4M
        CPU: 10.944s
     CGroup: /system.slice/kibana.service
             └─27053 /usr/share/kibana/bin/../node/... node ...
```

---

## 4. Create Elasticsearch enrollment token for Kibana

While on the VM, run:

```bash
root@MyDFIR-ELK:/usr/share/elasticsearch/bin# ./elasticsearch-create-enrollment-token --scope kibana
```

Save the generated token for the Kibana setup screen.

---

## 5. Open Kibana UI in browser

From your **laptop**, open:

`http://<VULTR_PUBLIC_IP>:5601`

You will see the **Kibana setup / enrollment** screen.

---

## 6. Firewall rules on Vultr

To allow Kibana access from your home/public IP:

- In Vultr:
    - **Settings → Firewall → Create/Edit Firewall Group.**
- Add **two rules** (do **not** open `1-65535` everywhere):

```bash
Action: Allow
Protocol: TCP
Port: 9200
Source: <YOUR_HOME_IP>/32

Action: Allow
Protocol: TCP
Port: 5601
Source: <YOUR_HOME_IP>/32
```

Apply that firewall group to the `MyDFIR-ELK` instance.

---

## 7. Optional: Local firewall on Ubuntu (ufw)

If you want to allow port 5601 on the Ubuntu server itself:

```bash
sudo ufw allow 5601
```

---

## 8. Finish Kibana enrollment

Back on the VM, copy the Kibana verification code:

```bash
root@MyDFIR-ELK:/usr/share/elasticsearch/bin# ./kibana-verification-code
```

Output:

```bash
Your verification code is:  *** ***
```

- Go to `http://<VULTR_PUBLIC_IP>:5601` in your laptop browser.
- Enter the **enrollment token**.
- Enter the **verification code** when prompted.

---

## 9. Login with Elastic superuser account

Use the credentials created during Elasticsearch installation:

- Username: `elastic`
- Password: `<ELASTIC_SUPERUSER_PASSWORD>`

---

## **10. Kibana Encryption Keys Setup**

### **Generate encryption keys**

```bash
root@MyDFIR-ELK:/usr/share/kibana/bin# ./kibana-encryption-keys generate
```

This command outputs three encryption keys:

- `xpack.encryptedSavedObjects.encryptionKey`
- `xpack.reporting.encryptionKey`
- `xpack.security.encryptionKey`

These keys are used to encrypt:

- saved objects (dashboards, visualizations, etc.),
- reports,
- user sessions/cookies.

### **Add keys to Kibana keystore**

```bash
root@MyDFIR-ELK:/usr/share/kibana/bin# ./kibana-keystore add xpack.encryptedSavedObjects.encryptionKey
Enter value for xpack.encryptedSavedObjects.encryptionKey: ****************************************************************
root@MyDFIR-ELK:/usr/share/kibana/bin# ./kibana-keystore add xpack.reporting.encryptionKey
Enter value for xpack.reporting.encryptionKey: ****************************************************************
root@MyDFIR-ELK:/usr/share/kibana/bin# ./kibana-keystore add xpack.security.encryptionKey
Enter value for xpack.security.encryptionKey: ****************************************************************
```

### Restart Kibana

```bash
root@MyDFIR-ELK:/usr/share/kibana/bin# systemctl restart kibana.service
```

Once Kibana restarts, your saved objects, reports, and sessions are now protected with Kibana‑managed encryption keys.