# Day 2: ELK Stack

After spending Day 1 mapping out the "blueprint" of my lab through **logical diagram**, it was time to dive into the engine that will power my entire security operation.

As someone aspiring to become a SOC analyst, I know that theory is great, but the goal of this challenge is to obtain **practical skills**. To do that, I need to understand the tools that real-world analysts use every day to hunt for threats.

### **What exactly is the ELK Stack?**

The "ELK" stack is a powerful collection of three open-source projects that work together to provide a centralized log management system. In our SOC lab, this stack acts as the "brain," collecting, storing, and visualizing every bit of telemetry we generate.

Here is how I broke down the components during today’s study:

- **Elasticsearch:** This is the heart of the stack. It’s a distributed search and analytics engine where all our data will be stored. Think of it as a massive, lightning-fast library of logs.
- **Logstash (and Beats/Agents):** This is the data processing pipeline. It’s responsible for ingesting data from multiple sources (like our Windows and Linux servers), transforming it, and sending it to Elasticsearch.
- **Kibana:** This is the part I’m most excited to get hands-on with. It’s the visualization layer—the web interface where I will eventually build **dashboards and alerts** to spot malicious activity.

### **Why Does a SOC Analyst Need ELK?**

One of the key lessons from today's introduction is that a SOC analyst cannot manually check every single computer in a network for signs of a breach. We need a way to **centralize that data**. By using the ELK stack, I can pull logs from a Windows Server in one corner of the network and an Ubuntu Server in another, viewing them all in one single pane of glass.