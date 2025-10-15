# Study of Malicious Software and Analysis Tools

**Purpose:** To understand the concept of malicious software (malware) and to study various tools used for detecting, removing, and preventing malicious activity on both Windows and Linux systems.

### Prerequisites

* A Windows computer with Administrator rights.
* A Linux environment (either a dedicated machine or WSL on Windows) with `sudo` rights.
* An internet connection.

---

### Part 1: Using MRT on Windows

**Tool:** MRT (Malicious Software Removal Tool)

**Purpose:** To scan for and remove specific, prevalent malware from a Windows system

#### Steps to Use MRT:

1.  **Open the Run Dialog:**
    * Press the `Windows Key + R` on your keyboard. A "Run" dialog box will appear.

2.  **Launch MRT:**
    * In the Run box, type `mrt` and press `Enter` or click "OK".
    * Click "Yes" on the User Account Control prompt.

3.  **Start the Scan:**
    * The MRT window will open. Click "Next" on the welcome screen.
    * You will be asked to choose a scan type:
        * **Quick scan:** Scans the areas of the system most likely to contain malware.
        * **Full scan:** Scans the entire system. This can take several hours.
        * **Customized scan:** Lets you choose a specific folder to scan.
    * Select "Quick scan" for this demonstration and click "Next".

4.  **View the Results:**
    * The tool will scan your system. Once it's done, it will show a result screen.
    * Hopefully, it will say "No malicious software was detected."[cite: 2463]. If malware is found, the tool will list and remove it.

---

### Part 2: Using Security Tools on Linux (Ubuntu / WSL)

For the next set of tools, you need to be in your Linux terminal.

#### 1. Hping3 - The Packet Crafter

**Tool:** Hping3

**Purpose:** A command-line tool for security auditing and testing firewalls by sending custom TCP/IP packets.

* **Installation:**
    ```bash
    sudo apt update
    sudo apt install hping3
    ```

* **Usage Example (SYN Flood Test):**
    > **Ethical Warning:** This command can be disruptive. **Only run it on your own machine's localhost (`127.0.0.1`) for safe testing.** Never use it on a website or server you do not own.

    This command sends a flood of `SYN` packets to port 80, a common way to test how a firewall handles a DoS attack.
    ```bash
    sudo hping3 -S --flood -p 80 127.0.0.1
    ```
    * Press `Ctrl + C` to stop the command after a few seconds.

#### 2. IPTables - The Linux Firewall

**Tool:** IPTables

**Purpose:** A user-space utility to configure the IP packet filter rules of the Linux kernel firewall.

* **Usage Examples:**

    * **List Current Rules:** This command shows all the current rules in your firewall.
        ```bash
        sudo iptables -L
        ```

    * **Block an IP Address:** This is how you can block all incoming traffic from a specific malicious IP address.
        ```bash
        # Replace 192.168.1.100 with the IP you want to block
        sudo iptables -A INPUT -s 192.168.1.100 -j DROP
        ```
        * `-A INPUT`: Appends a rule to the INPUT chain.
        * `-s 192.168.1.100`: Specifies the source IP address.
        * `-j DROP`: Tells the firewall to drop the packet.

#### 3. Nethogs - The Process Network Monitor

**Tool:** Nethogs

**Purpose:** A network monitoring tool that shows bandwidth usage per process, helping to identify which programs are using your network.
* **Installation:**
    ```bash
    sudo apt install nethogs
    ```

* **How to Run:** You need to specify your network interface name (like `eth0` or `wlan0`).
    1.  First, find your interface name by running `ip addr`.
    2.  Then, run Nethogs with that name.
        ```bash
        # Replace 'eth0' with your actual network interface name
        sudo nethogs eth0
        ```

* **Understanding the Output:**
    * Nethogs will display a live table showing the `PID` (Process ID), `USER`, `PROGRAM`, and the `SENT` and `RECEIVED` data rate for each process using the network.
    * This is extremely useful for spotting any unknown program that is sending or receiving a lot of data, which could be a sign of malware. Press `q` to quit.

---

### Conclusion

This experiment demonstrates a multi-layered approach to security. Tools like **MRT** help in cleaning existing infections. Firewall tools like **IPTables** provide a proactive defense by blocking malicious traffic. Finally, monitoring tools like **Hping3** and **Nethogs** are essential for testing defenses and detecting abnormal activity in real-time.
