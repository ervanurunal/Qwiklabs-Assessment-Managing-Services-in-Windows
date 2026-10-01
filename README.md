## Qwiklabs Assessment Managing Services in Windows
---
### Overview

In this lab, I practiced managing services on a Windows machine. I learned how to:

* List Windows services
* Check the status of services
* Start and stop services
* Enable disabled services
* Manage services using PowerShell
* Install additional Windows features
* Configure IIS
* Create and serve a website
* Verify that a web service is running

These are important IT support and system administration skills because services are responsible for many background functions and applications running on Windows.

---

### Tools & Resources

* Windows Virtual Machine
* Windows Services Console
* Windows PowerShell
* IIS (Internet Information Services)
* Server Manager
* Windows Service Management Commands
* Qwiklabs

### Lab Type: 
Windows / IT Support Hands-On Lab

---
---

## 1. List Windows System Services

Windows provides a **Services** application that allows administrators to view and manage system services.

### Open Services

The Services console can be opened through:

**Control Panel → System and Security → Administrative Tools → Services**

![1](https://i.imgur.com/Di9CiSZ.png)

![2](https://i.imgur.com/8C1a4ov.png)

![3](https://i.imgur.com/cO7bOtl.png)

![4](https://i.imgur.com/rpYFbHV.png)

The Services console displays information such as:

* Service name
* Description
* Status
* Startup configuration

Services can also be sorted by their current status.

![5](https://i.imgur.com/2a0JTUJ.jpeg)

---

## 2. Start and Stop Services Using the Services Console

Windows allows administrators to control services directly from the Services application.

When I right-click a service, I can see options such as:

* Start
* Stop
* Pause
* Resume
* Restart
* Properties
* Refresh

Unavailable actions are grayed out depending on the current service state.

![6](https://i.imgur.com/bFY7w3g.jpeg)

---

## 3. Stop the Themes Service

For this lab, I practiced stopping the **Themes** service.

### Steps

1. Open the **Services** application.
2. Locate **Themes**.
3. Right-click the service.
4. Select **Stop**.
5. Wait for the service to stop.

![7](https://i.imgur.com/hUXrHvW.jpeg)

![8](https://i.imgur.com/W4sXWSE.jpeg)

---

### IT Support Relevance

Stopping and restarting services is a common troubleshooting technique when an application or Windows component is not functioning correctly.

---

## 4. Start the Performance Logs & Alerts Service

The **Performance Logs & Alerts** service can collect performance-related information and trigger alerts when configured thresholds are reached.

### Steps

1. Locate **Performance Logs & Alerts**.
2. Right-click the service.
3. Select **Start**.
4. Verify that the service is running.

![9](https://i.imgur.com/nPV4vbn.jpeg)

![10](https://i.imgur.com/11qyNF5.png)

### Why This Matters

Performance monitoring can help administrators identify:

* High resource usage
* Performance problems
* System bottlenecks
* Threshold violations

---

## 5. Configure a Service Through Windows Settings

Some services can also be controlled indirectly through Windows settings.

For example, the **Auto Time Zone Updater** service is related to the Windows setting:

**Set time zone automatically**

### Steps

1. Open **Date and Time settings**.

![11](https://i.imgur.com/RgVhsdB.png)

2. Enable **Set time zone automatically**.

![12](https://i.imgur.com/ZofqIUh.png)

3. Return to the Services application.
4. Refresh the service list.
5. Verify the service configuration.

![13](https://i.imgur.com/qe8snrZ.png)

---

## 6. Manage Services with PowerShell

GUI tools are useful for manual administration, but system administrators often use the command line to automate repetitive tasks.

I opened **Windows PowerShell** and used PowerShell service-management commands.

---

## 7. List Services with `Get-Service`

The PowerShell command used to list Windows services is:

```powershell
Get-Service
```

This command displays information including:

* Status
* Service name
* Display name

![14](https://i.imgur.com/rJMnmh5.jpeg)

---

## 8. Get Detailed Service Information

PowerShell can provide additional information about a specific service.

Example from the lab:

```powershell
Get-Service wisvc | Format-List
```

This provides detailed information about the **wisvc** service.

The short service name `wisvc` corresponds to the **Windows Insider Service**.

![15](https://i.imgur.com/9Qs6yNM.png)

---

## 9. Start a Windows Service

PowerShell provides the `Start-Service` command.

### Syntax

```powershell
Start-Service wisvc
```

The command may not produce output when successful.

I can verify the result with:

```powershell
Get-Service wisvc
```

![16](https://i.imgur.com/zdQYGpd.png)

The lab demonstrates starting the Windows Insider Service and then verifying its status.

---

## 10. Stop a Windows Service

PowerShell can also stop services using:

```powershell
Stop-Service wisvc
```

After stopping the service, I can verify its status:

```powershell
Get-Service wisvc
```

![17](https://i.imgur.com/7WR4eZh.png)

This confirms whether the service is running or stopped.

---

## 11. Enable a Disabled Service

Not every service can be started immediately.

The lab uses the **Smart Card** service (`ScardSvr`) as an example.

First, I checked its configuration:

```powershell
Get-Service ScardSvr | Format-List *
```

The important field is:

```text
StartType
```

If the StartType is:

```text
Disabled
```

the service cannot be started until its startup configuration is changed.

![18](https://i.imgur.com/3FIrUNI.png)

---

## 12. Change the Service Startup Type

The lab uses `Set-Service` to modify the service configuration.

### Example

```powershell
Set-Service ScardSvr -StartupType Manual
```

After changing the startup type, the service can be started.

```powershell
Start-Service ScardSvr
```

Then verify:

```powershell
Get-Service ScardSvr
```

![18](https://i.imgur.com/WkPESY2.png)

The lab demonstrates enabling and starting the service successfully.

---

## 13. Enable Additional Windows Features

Windows includes features that are not necessarily enabled by default.

The lab uses PowerShell to install additional web-serving functionality.

The command used is:

```powershell
Install-WindowsFeature
```

![19](https://i.imgur.com/jvh6tff.png)

The feature installation may take several minutes because Windows needs to download and install additional components.

---

## 14. IIS Web Server

After enabling the web-serving features, additional services become available.

One important service is:

```text
IISADMIN
```

![20](https://i.imgur.com/6pVVkQn.png)

This service is associated with publishing websites on the machine.

---

## 15. Configure IIS

I opened:

**Internet Information Services (IIS) Manager**

### Steps

1. Search for **IIS** in the Windows Start menu.

![21](https://i.imgur.com/tYvy5iU.png)

2. Open **Internet Information Services (IIS) Manager**.
3. Expand the server.
4. Select **Sites**.
5. Locate the existing **Default Website**.
6. Right-click and select **Add Website**.

![22](https://i.imgur.com/TfNaGuZ.png)

---

## 16. Create a New Website

When adding a website, I configured:

* Website name
* Physical path
* Port

The lab uses the following website directory:

```text
C:\Users\qwiklabs\amazingsite
```

The website was configured to use:

```text
Port 80
```

![23](https://i.imgur.com/1V2b0zp.png)

---

## 17. Verify the Website

After creating the website, I verified that it was accessible through the machine's **External IP address**.

The website was configured to serve the content from the selected physical directory.

This confirmed that the IIS web server was running and serving web content successfully.

![24](https://i.imgur.com/Vknejli.png)

---
---

## Cybersecurity Relevance

Understanding Windows services is useful for both **IT Support** and **SOC Analyst** roles.

### 1. Service Monitoring

Security analysts may need to identify unusual or unexpected services running on a system.

```powershell
Get-Service
```

can help provide an initial view of available services.

### 2. Suspicious Services

Unexpected services can potentially indicate:

* Unauthorized software
* Persistence mechanisms
* Malware
* Misconfiguration

### 3. Service State Investigation

Knowing how to determine whether a service is:

* Running
* Stopped
* Disabled
* Enabled

helps during system troubleshooting and security investigations.

### 4. PowerShell Skills

PowerShell is an important Windows administration and automation tool.

Understanding commands such as:

```powershell
Get-Service
Start-Service
Stop-Service
Set-Service
```

provides a foundation for investigating and managing Windows systems.

### 5. Web Server Awareness

IIS is a common Windows web-server technology. Understanding how a website is configured helps when investigating:

* Web server activity
* Service failures
* Configuration changes
* Unexpected web services

---
---

## Key Concepts Learned

| Concept          | Description                                                                    |
| ---------------- | ------------------------------------------------------------------------------ |
| Service          | A background Windows process that provides system or application functionality |
| Services Console | GUI used to manage Windows services                                            |
| PowerShell       | Command-line environment for Windows administration                            |
| `Get-Service`    | Lists and retrieves service information                                        |
| `Start-Service`  | Starts a service                                                               |
| `Stop-Service`   | Stops a service                                                                |
| `Set-Service`    | Changes service configuration                                                  |
| `Format-List`    | Displays detailed object information                                           |
| StartType        | Determines how a service can start                                             |
| IIS              | Internet Information Services web server                                       |
| Port 80          | Port used by the website configured in this lab                                |

---
---

## Troubleshooting Workflow

When troubleshooting a Windows service:

```text
Identify the service
       ↓
Check the service status
       ↓
Review the service configuration
       ↓
Determine whether the service should be running
       ↓
Start / Stop / Restart if appropriate
       ↓
Check for errors
       ↓
Verify the final status
```

For a disabled service:

```text
Check StartType
       ↓
If Disabled
       ↓
Change StartupType
       ↓
Start the service
       ↓
Verify status
```

---
---

## Important PowerShell Commands

```powershell
# List all services
Get-Service

# Get detailed information
Get-Service -Name "ServiceName" | Format-List

# Start a service
Start-Service -Name "ServiceName"

# Stop a service
Stop-Service -Name "ServiceName"

# Change service startup configuration
Set-Service -Name "ServiceName" -StartupType Manual

# Install Windows features
Install-WindowsFeature
```

---
---

## Skills Demonstrated

* Windows Service Management
* Windows Troubleshooting
* PowerShell
* System Administration
* Service Monitoring
* Service Configuration
* IIS Configuration
* Web Server Management
* Command-Line Administration
* Basic Security Monitoring Concepts

---
---

## Final Takeaway

This lab helped me build practical experience managing Windows services through both the graphical **Services console** and **PowerShell**.

I practiced checking service states, starting and stopping services, enabling a disabled service, installing additional Windows features, and configuring IIS to serve a website.

These skills provide a useful foundation for **IT Support, System Administration, and SOC Analyst** work because security and troubleshooting often require understanding what services are running on a Windows system and how those services are configured.
