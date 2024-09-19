
---
title: "Windows OS fundemtals  "
summary: "Windows OS fundemtals and basics for soc and dfir analyst and cs studetns like permessions and users , groups , firewalls , logs based on letsdefend module in a cheat sheet style  "
date: "2024-09-19T00:00:00Z"
tags:
  - Windows
  - OS
  - Education
  - SOCK
  - DFIR
  
author: "Katana"
description: "Learn about the Windows OS fundemtals and basics for soc and dfir analyst and cs studetns like permessions and users , groups , firewalls , logs based on letsdefend module in a cheat ."
canonicalURL: "https://h3xkatana.github.io/Blog/post/windows-basics"
categories:
  - Windows
  - SOC 
  - Cheatsheet
  - Education
showToc: true
tocOpen: true
---
# Windows Basics

The Windows command line is an essential tool that allows users to interact with the operating system through typed commands. This guide provides an overview of basic Windows command line operations, network commands, file operations, permissions, process management, services, scheduled tasks, and security features like the firewall and event logs.

### Basic Commands:

1. **help**: Displays information about available commands.
2. **dir**: Lists files and directories in the current folder.
3. **cd**: Changes the current directory.
4. **date**: Displays or changes the system date.
5. **echo**: Prints messages to the screen.
6. **hostname**: Shows the system's hostname.
7. **time**: Displays or changes the system time.

### Network Commands:

1. **ipconfig**: Displays network interface details.
2. **netstat**: Shows active network connections and their status.
3. **nslookup**: Resolves domain names to IP addresses.
4. **ping**: Tests network communication between devices.

### File Operations:

1. **type**: Displays the contents of a file.
2. **copy**: Copies files to a new location.
3. **mkdir**: Creates a new directory.
4. **rename**: Renames files.
5. **move**: Moves files between directories.
6. **tree**: Lists directory structures (`/a /F` for a full recursive tree).
7. **rmdir**: Deletes directories.

---

## Windows Permissions Management

**Permissions Management** is crucial for maintaining security in Windows. Each file and folder inherits permissions from its parent directory, which controls user access.

- **File and Folder Permissions**: Windows assigns specific permissions to files and folders, such as **Read**, **Write**, or **Execute**, preventing unauthorized access. These permissions can be managed via the "Security" tab in a file’s properties.
- **Permission Types**: The six main permission types are **Full Control**, **Modify**, **Read & Execute**, **Read**, **Write**, and **Special Permissions**.
- **Changing Permissions**: Only the file owner or an administrator can change permissions. Denying a permission, like **Read**, will restrict access.
- **User Account Control (UAC)**: UAC adds a layer of security by prompting for admin approval when changes are made. However, UAC can be bypassed by attackers, so it's important not to rely solely on it.

---

## User and Group Management

Windows uses users and groups to define access levels and privileges. Attackers often target high-privilege users like administrators. Monitoring user activity is essential for detecting suspicious behavior.

- **whoami**: Displays the current user account.
- **net user**: Lists user accounts and their details.
- **net accounts**: Shows password usage and login restrictions for all users.
- **net localgroup**: Manages groups, such as displaying members of the "Administrators" group.

For a graphical interface, use **lusrmgr.msc** to manage users and groups, add new accounts, or modify group membership.

---

## Windows Process Management

Processes are running programs, each with a unique Process ID (PID). Processes may create child processes, forming a hierarchical structure known as the **process tree**. Monitoring these processes is critical for maintaining system security.

### Key Legitimate Windows Processes:

- **wininit.exe**: Starts system services and security components.
- **services.exe**: Manages system services.
- **svchost.exe**: Runs DLL-based services.
- **lsass.exe**: Handles authentication and is a target for password extraction attacks (e.g., with **Mimikatz**).
- **explorer.exe**: Manages the graphical user interface (GUI).

### Process Commands:

- **tasklist**: Lists all running processes.
- **taskkill**: Terminates processes using their PID.
- **findstr**: Filters command output (e.g., `tasklist | findstr`).

---

## Windows Services Management

Windows **Services** run in the background to perform essential system tasks. Monitoring and managing these services is crucial for both system functionality and security.

### Managing Services via GUI:

1. Open **Services** (`Windows + R`, type `services.msc`).
2. View or manage services by right-clicking on any service and selecting **Properties**.
3. You can start, stop, or restart services from this window.

### Managing Services via Command Line:

1. **List services**: `sc query`
2. **List all services (running or not)**: `sc query type=service state=all`
3. **Start a service**: `sc start <service_name>`
4. **Stop a service**: `sc stop <service_name>`

---

## Task Scheduler

**Task Scheduler** automates tasks at predefined intervals or conditions. However, attackers may use it for persistence. Monitoring scheduled tasks is essential for detecting unauthorized activities.

### Managing Scheduled Tasks via GUI:

1. Open **Task Scheduler** (`Windows + R`, type `taskschd.msc`).
2. View tasks under the **Task Scheduler Library**.
3. Create, modify, or delete tasks as needed.

### Managing Scheduled Tasks via Command Line:

1. **List tasks**: `schtasks`
2. **View a specific task**: `schtasks /Query /TN <TaskName>`
3. **Run a task**: `schtasks /Run /TN <TaskName>`
4. **Delete a task**: `schtasks /Delete /TN <TaskName>`

---

## Windows Registry Overview

The **Windows Registry** is a hierarchical database that stores system and application settings. Attackers often exploit the registry for persistence. Monitoring registry changes is important for detecting malicious activity.

### Accessing and Managing the Registry:

1. **Open Registry Editor** (`Windows + R`, type `regedit`).
2. Registry entries are organized into keys and values, such as **HKEY_LOCAL_MACHINE** (HKLM) for system-wide settings.
3. You can also manage the registry via the command line with the `reg` command (e.g., `reg query HKEY_LOCAL_MACHINE\\SYSTEM\\...`).

---

## Windows Firewall

**Windows Firewall** controls incoming and outgoing network traffic. It is essential for blocking unauthorized access while allowing secure connections.

### Managing Firewall Rules via GUI:

- Open **Windows Firewall** to view and manage inbound/outbound rules.
- You can create custom rules, such as blocking traffic on specific ports.

### Managing Firewall Rules via Command Line:

1. **List all rules**: `netsh advfirewall firewall show rule name=all`
2. **View a specific rule**: `netsh advfirewall firewall show rule name="<rule_name>"`
3. **Delete a rule**: `netsh advfirewall firewall delete rule name="<rule_name>"`

---

## Event Logs

**Event Logs** record system, application, and security activities, such as login attempts or service changes. Analyzing event logs is vital for identifying cyberattacks and system malfunctions.

### Viewing Event Logs:

1. Open **Event Viewer** (`Windows + R`, type `eventvwr`).
2. Event logs are categorized by type (e.g., security, system, application) and can be filtered by **Event ID** for easier analysis.

---

## Windows Management Instrumentation (WMI)

**WMI** allows administrators to query and manage systems remotely, but attackers can exploit WMI for reconnaissance and lateral movement. Monitoring WMI activities is key to identifying unauthorized actions.

### Common WMI Commands:

- **Get OS details**: `wmic os list brief`
- **List users**: `wmic useraccount get name`