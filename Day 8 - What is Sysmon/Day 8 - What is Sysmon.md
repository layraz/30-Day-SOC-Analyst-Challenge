# Day 8: What is Sysmon?

**Objective**

The primary goal of Day 8 is to understand the capabilities of **System Monitor (Sysmon)**, a Windows system service and device driver that monitors and logs system activity to the Windows event log. This foundational knowledge is a prerequisite for the **Sysmon Setup Tutorial** occurring on Day 9.

**Why Sysmon is Essential for a SOC Analyst**

While standard Windows Event Logs provide basic information, Sysmon offers the granular data needed to track sophisticated adversary behavior. Key areas of focus during this introductory phase include:

- **Detailed Process Tracking:** Recording process creation with full command lines and parent-child relationships.
- **Network Connection Monitoring:** Identifying which processes are initiating network traffic, which is vital for detecting **Command and Control (C2)** activity later in the challenge.
- **High-Fidelity Logs:** Providing a much deeper level of visibility than default logging, which is necessary for effective **incident investigations**

| ID | Tag | Event |
| --- | --- | --- |
| **1** | ProcessCreate | Process Create |
| **2** | FileCreateTime | File creation time |
| **3** | NetworkConnect | Network connection detected |
| **4** | n/a | Sysmon service state change (cannot be filtered) |
| **5** | ProcessTerminate | Process terminated |
| **6** | DriverLoad | Driver Loaded |
| **7** | ImageLoad | Image loaded |
| **8** | CreateRemoteThread | CreateRemoteThread detected |
| **9** | RawAccessRead | RawAccessRead detected |
| **10** | ProcessAccess | Process accessed |
| **11** | FileCreate | File created |
| **12** | RegistryEvent | Registry object added or deleted |
| **13** | RegistryEvent | Registry value set |
| **14** | RegistryEvent | Registry object renamed |
| **15** | FileCreateStreamHash | File stream created |
| **16** | n/a | Sysmon configuration change (cannot be filtered) |
| **17** | PipeEvent | Named pipe created |
| **18** | PipeEvent | Named pipe connected |
| **19** | WmiEvent | WMI filter |
| **20** | WmiEvent | WMI consumer |
| **21** | WmiEvent | WMI consumer filter |
| **22** | DnsQuery | DNS query |
| **23** | FileDelete | File Delete archived |
| **24** | ClipboardChange | New content in the clipboard |
| **25** | ProcessTampering | Process image change |
| **26** | FileDeleteDetected | File Delete logged |
| **27** | FileBlockExecutable | File Block Executable |
| **28** | FileBlockShredding | File Block Shredding |
| **29** | FileExecutableDetected | File Executable Detected |