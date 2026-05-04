# Day 6: Elastic Agent and Fleet Server Introduction

### **Day 6: Elastic Agent and Fleet Server Introduction**

Following the successful installation of the **Windows Server 2022** on Day 5, the challenge moves to the conceptual foundation of telemetry collection: the **Elastic Agent and Fleet Server Introduction**. This day is essential for understanding how the lab will transition from isolated servers into a monitored ecosystem.

### **Key Concepts**

- **Elastic Agent:** This is a single, unified agent used to collect logs, metrics, and security data from a host. It replaces the need for multiple standalone "Beats" (like Winlogbeat or Metricbeat) by providing a single installer that can be managed remotely.
- **Fleet Server:** This component acts as the central management hub. It allows the analyst to manage all deployed Elastic Agents from a single interface in Kibana, enabling the remote update of configurations and security policies.

### **Why This Matters for a SOC**

In a professional SOC environment, manual configuration of individual endpoints is not scalable. Day 6 introduces the architecture required to:

- **Centralize Control:** Change what data is being collected across the entire lab without logging into each machine individually.
- **Standardize Telemetry:** Ensure that both the Windows and future Linux hosts follow the same logging policies.