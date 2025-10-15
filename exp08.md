# Analyzing Network Traffic with Wireshark

**Purpose:** To study the use of a packet sniffer tool by installing Wireshark and using it to capture and analyze different types of network protocols, including TCP, UDP, and HTTP.

### Prerequisites

* A computer running Windows with Administrator rights (for installation).
* An active internet connection (Wi-Fi or Ethernet).

---

### Step 1: Download and Install Wireshark

First, we need to get the tool on our system.

1.  **Download Wireshark:**
    * Open your web browser and go to the official Wireshark download page: [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
    * Download the "Windows Installer (64-bit)" for your system.

2.  **Install Wireshark:**
    * Find the downloaded installer file (e.g., `Wireshark-x.x.x-x64.exe`) in your `Downloads` folder and double-click it.
    * Click "Yes" on the User Account Control prompt.
    * Follow the setup wizard. You can leave all the default components selected and click "Next" through all the options.
    * **Important:** During the installation, it will ask to install `Npcap`. Make sure the box for `Npcap` is checked and proceed with its installation. `Npcap` is the essential component that allows Wireshark to capture live network traffic.
    * Once both `Npcap` and Wireshark are installed, click "Finish".

---

### Step 2: Capturing Live Network Traffic

Now let's capture some data packets from your network.

1.  **Open Wireshark:** Find Wireshark in your Start Menu and open it. You may need to run it as an administrator for the best results.

2.  **Select the Network Interface:**
    * The Wireshark home screen will show a list of available network interfaces.
    * Look for the one that has activity (a fluctuating line graph next to its name). This will likely be **"Wi-Fi"** if you are on wireless, or **"Ethernet"** if you are connected with a cable.

3.  **Start the Capture:**
    * Double-click on your active network interface name.
    * Wireshark will immediately start capturing all the data packets moving through that interface. You will see a constantly updating list of packets.

4.  **Generate Some Traffic:**
    * While Wireshark is capturing, open your web browser and visit a simple, non-secure website like `http://info.cern.ch/` (the world's first website). Using a non-secure (HTTP) site makes it easier to see the data.

5.  **Stop the Capture:**
    * Go back to the Wireshark window and click the **red square button** (Stop packet capture) near the top-left corner.

---

### Step 3: Analyzing the Captured Packets

Now that we have some data, let's filter it to understand different protocols.

#### 1. Analyzing HTTP Traffic

HTTP is the protocol used to browse websites.

* **Apply a Filter:** In the "Apply a display filter..." bar at the top, type `http` and press `Enter`. Wireshark will now only show the HTTP packets from your capture.
* **Inspect a Packet:**
    * Find a packet in the list that says "GET /..." in the "Info" column. This is your browser requesting the webpage.
    * Click on it. In the middle panel, expand the "Hypertext Transfer Protocol" section. You can see the Host, User-Agent, and other details of the request your browser sent.
    * **Follow Stream:** Right-click on the HTTP packet and select `Follow` -> `TCP Stream`. A new window will pop up showing the entire conversation between your browser (in red) and the server (in blue), including the raw HTML of the webpage.

#### 2. Analyzing TCP Traffic

TCP is a reliable protocol used by HTTP, email, and many other applications.

* **Apply a Filter:** Change the filter to `tcp` and press `Enter`.
* **Inspect a Packet:**
    * Look for the first three packets of a conversation. You can often see `[SYN]`, `[SYN, ACK]`, and `[ACK]` in the "Info" column. This is the **TCP three-way handshake** used to establish a connection.
    * Click on any TCP packet. In the middle panel, expand the "Transmission Control Protocol" section. You can see details like Source Port, Destination Port, and Sequence/Acknowledgment numbers.

#### 3. Analyzing UDP Traffic

UDP is a faster, connectionless protocol used for things like video streaming or DNS lookups.

* **Apply a Filter:** Change the filter to `udp` and press `Enter`.
* **Inspect a Packet:**
    * Click on a UDP packet. In the middle panel, expand the "User Datagram Protocol" section.
    * You will notice that it's much simpler than TCP. There are no sequence numbers or handshake flags because UDP does not guarantee delivery.

---

### Conclusion

By using Wireshark, you have successfully captured live network traffic and analyzed it at the packet level. You have learned to filter for specific protocols like HTTP, TCP, and UDP, and inspect their headers and data. This experiment provides a foundational understanding of how data moves across a network and how tools like Wireshark are essential for network analysis and security.
