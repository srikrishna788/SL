# Network Reconnaissance Tools Guide

**Purpose:** To study and use various fundamental network reconnaissance tools to gather information about a target network. This guide covers domain registration details, IP addresses, network paths, and active services using standard command-line utilities.

### Prerequisites

* A computer with an internet connection.
* Administrator/sudo rights to install new software.

---

## Environment 1: Performing on Windows using WSL (Windows Subsystem for Linux)

This method allows you to run a full Linux terminal directly on your Windows machine, which is ideal for using these tools.

### Step 1: Set Up the Linux Environment on Windows

If you already have WSL with Ubuntu installed, you can skip to Step 2.

1.  **Open PowerShell as Administrator:**
    * Click the Start Menu, type `PowerShell`.
    * Right-click on "Windows PowerShell" and select "Run as administrator".

2.  **Install WSL and Ubuntu:**
    * In the PowerShell window, type the following command and press `Enter`[cite: 931]. This will download and install the Windows Subsystem for Linux along with the Ubuntu distribution.
        ```powershell
        wsl --install
        ```
    * After the installation is complete, **restart your computer**.

3.  **Launch Ubuntu:**
    * After restarting, click the Start Menu and find the "Ubuntu" application. Click to open it.
    * The first time it runs, it will ask you to create a username and password for your new Linux environment. Remember these details.

### Step 2: Install and Use the Reconnaissance Tools in WSL

All the following commands should be run inside the **Ubuntu terminal**.

1.  **Update and Install Tools:**
    * First, update your package list. Then, install `whois`, `dnsutils` (which contains `dig` and `nslookup`), `traceroute`, and `nmap`.
        ```bash
        sudo apt update
        sudo apt install whois dnsutils traceroute nmap
        ```
    * You will need to enter the password you created for Ubuntu.

2.  **Use the Tools:**

    * **`whois` - Find Domain Ownership**
        * **Purpose:** Retrieves registration information for a domain name[cite: 885].
        * **Command:**
            ```bash
            whois google.com
            ```
        * **What it shows:** Details like the domain's creation date, expiry date, and registrar[cite: 955, 960, 961, 962].

    * **`dig` - DNS Lookup**
        * **Purpose:** Queries DNS servers for records[cite: 892].
        * **Command:**
            ```bash
            dig google.com
            ```
        * **What it shows:** Look for the "ANSWER SECTION" to find the IP address associated with `google.com` (e.g., `142.250.70.78`)[cite: 1009, 1018].

    * **`nslookup` - Name Server Lookup**
        * **Purpose:** Another tool for DNS queries[cite: 892].
        * **Command:**
            ```bash
            nslookup google.com
            ```
        * **What it shows:** Provides the IP addresses (both IPv4 and IPv6) for the domain[cite: 1051, 1052, 1055].

    * **`ping` - Check Host Availability**
        * **Purpose:** Sends packets to test if a host is reachable and measures response time[cite: 896].
        * **Command:**
            ```bash
            ping google.com
            ```
        * **What it shows:** A series of replies from the server, including the `time=` value, which represents the latency. Press `Ctrl + C` to stop it.

    * **`traceroute` - Map the Network Path**
        ***Purpose:** Shows the "hops" or path your data takes to reach a destination server[cite: 898].
        * **Command:**
            ```bash
            traceroute google.com
            ```
        * **What it shows:** A list of all the routers your packet passes through on its way to `google.com`[cite: 1099, 1100, 1109].

    * **`nmap` - The Network Mapper**
        * **Purpose:** A powerful tool for discovering hosts and open ports/services on a network[cite: 902].
        * **Command (Safe Example):** Use Nmap's official test site.
            ```bash
            nmap scanme.nmap.org
            ```
        * **What it shows:** A list of open ports and the services running on them (e.g., `22/tcp open ssh`, `80/tcp open http`).

---

## Environment 2: Performing on a Native Ubuntu Desktop

If your lab machine is already running Ubuntu, the process is even simpler.

### Step 1: Open the Terminal

* You can open the terminal by pressing the keyboard shortcut `Ctrl + Alt + T`.
* Alternatively, click the "Show Applications" icon (usually a grid of dots in the bottom-left) and search for "Terminal".

### Step 2: Install and Use the Reconnaissance Tools

Most of these tools are standard on Linux, but it's good practice to ensure they are installed.

1.  **Update and Install Tools:**
    * Open your terminal and run the following command to update your system and install all the necessary tools at once.
        ```bash
        sudo apt update && sudo apt install whois dnsutils traceroute nmap -y
        ```
    * Enter your password when prompted. The `-y` flag automatically confirms the installation.

2.  **Use the Tools:**
    * The commands to use the tools and the output you will see are **exactly the same** as in the WSL environment. Please refer to the commands listed in **"Environment 1, Step 2"** above to perform your reconnaissance tasks.

    * **Quick Command Reference:**
        * `whois google.com`
        * `dig google.com`
        * `nslookup google.com`
        * `ping google.com` (Stop with `Ctrl + C`)
        * `traceroute google.com`
        * `nmap scanme.nmap.org`

---
