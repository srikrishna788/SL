# Network Scanning with Nmap

**Purpose:** To download, install, and use the Nmap tool with various options. This experiment demonstrates how to perform fundamental network scanning tasks like host discovery (ping scan), port scanning, and operating system (OS) fingerprinting.

### Prerequisites

* A computer running either Windows or a Linux distribution (like Ubuntu/WSL).
* Administrator or `sudo` rights to install software and run certain scans.
* An internet connection.

---

### Step 1: Download and Install Nmap

The installation process is different for Windows and Linux.

#### On Windows:
1.  **Download the Installer:**
    * Go to the official Nmap download page: [https://nmap.org/download.html](https://nmap.org/download.html)
    * Find the "Windows binaries" section and download the "Latest stable release self-installer" (e.g., `nmap-x.xx-setup.exe`).
2.  **Run the Installer:**
    * Double-click the downloaded file.
    * Follow the setup wizard, accepting the license agreement and leaving all default components (like `Npcap`) selected. Click "Install" and "Finish". Nmap will be automatically added to your system's PATH, so you can run it from Command Prompt.

#### On Ubuntu / WSL:
1.  **Open the Terminal.**
2.  **Install Nmap:** Run the following commands to update your package list and install Nmap.
    ```bash
    sudo apt update
    sudo apt install nmap
    ```

---

### Step 2: Performing Scans with Nmap

All the following commands should be run from your **Command Prompt (on Windows)** or **Terminal (on Ubuntu/WSL)**.

> **Ethical Warning:**
> **IMPORTANT!** Never run Nmap scans on websites or networks you do not own or have explicit written permission to test. For this experiment, we will **ONLY** use `scanme.nmap.org`, which is a server provided by the Nmap developers for safe and legal scanning practice.

#### Scan 1: Host Discovery (Ping Scan)

This scan quickly determines which hosts on a network are online.

* **Purpose:** To see if a target machine is up and running.
* **Command:**
    ```bash
    nmap -sn scanme.nmap.org
    ```
* **Understanding the Output:** The output will be very simple. It will tell you that the "Host is up" and may also show its latency, confirming that the target is reachable.

#### Scan 2: TCP Port Scan

This scan checks for open TCP ports on a target machine.

* **Purpose:** To find out which services (like a web server or SSH) are running and accessible.
* **Command:**
    ```bash
    nmap -sT scanme.nmap.org
    ```
* **Understanding the Output:** The output will show a list of ports, their state (open, closed, or filtered), and the common service associated with that port. For example:
    ```
    PORT   STATE  SERVICE
    22/tcp open   ssh
    80/tcp open   http
    ```
    This means the target is likely running an SSH server (port 22) and a web server (port 80).

#### Scan 3: OS Fingerprinting (Operating System Detection)

This scan attempts to guess the operating system of the target host.

* **Purpose:** To identify the OS running on the target machine, which can be useful for security assessments.
* **Note:** This scan is more intrusive and often requires administrator/root privileges.
* **Command:**
    * On Windows, open Command Prompt **as Administrator**.
    * On Linux, use `sudo`.
    ```bash
    nmap -O scanme.nmap.org
    ```
    (If you are on Linux, use `sudo nmap -O scanme.nmap.org`)
* **Understanding the Output:** Along with the open ports, Nmap will provide details about the likely operating system. You might see lines like:
    ```
    Running: Linux 2.6.X
    OS details: Linux 2.6.32 - 3.10
    ```
    This indicates that the target server is running a Linux kernel.

---

### Conclusion

Nmap is an essential tool for any network administrator or security professional. Through this experiment, you have learned how to use its basic but powerful features to perform host discovery (`-sn`), identify open services with a port scan (`-sT`), and even determine the operating system of a target machine (`-O`). These are the foundational skills for network exploration and security auditing.
