# WireGuard with Algo VPN Setup Instructions

## Prerequisites
- A server or cloud instance to run Algo VPN (e.g., DigitalOcean, AWS).
- A client device with WireGuard installed (available for Windows, macOS, Linux, iOS, and Android).

## Installation

### 1. Set Up Algo VPN
1. **Clone the Algo Repository:**
    ```bash
    git clone https://github.com/trailofbits/algo.git
    cd algo
    ```
2. **Install Dependencies:**
    - On Ubuntu/Debian:
        ```bash
        sudo apt-get update && sudo apt-get install -y python3-virtualenv
        ```
    - On macOS:
        ```bash
        brew install python3
        ```
3. **Configure Algo VPN:**
    - Create a Python virtual environment and activate it:
        ```bash
        python3 -m virtualenv env
        source env/bin/activate
        ```
    - Install Algo’s dependencies:
        ```bash
        pip install -r requirements.txt
        ```
    - Run the Algo VPN setup script:
        ```bash
        ./algo
        ```
    - Follow the prompts to configure your VPN, choosing WireGuard as the VPN protocol.

4. **Deploy Algo VPN:**
    - Choose a cloud provider (e.g., DigitalOcean, AWS, GCE) and follow the prompts to deploy the VPN server.
    - Once deployed, Algo will generate configuration files for your VPN clients.

## Configuration

### 2. Configure WireGuard Client
1. **Download Configuration Files:**
    - Copy the generated WireGuard configuration files (usually located in the `configs` directory) to your client device.

2. **Install WireGuard:**
    - On Windows:
        - Download and install WireGuard from [the official site](https://www.wireguard.com/install/).
    - On macOS:
        ```bash
        brew install wireguard-tools
        ```
    - On Linux:
        ```bash
        sudo apt-get install wireguard
        ```
    - On iOS/Android:
        - Download and install WireGuard from the App Store or Google Play.

3. **Add the Configuration:**
    - Import the configuration file into the WireGuard client.

4. **Start the VPN:**
    - Activate the WireGuard VPN connection using the client app or command-line tool.

## Testing

### 1. Verify VPN Connection

#### Check IP Address
1. **Before Connecting to VPN:**
    - Go to a website like [WhatIsMyIP](https://www.whatismyip.com/) or use the `curl` command in a terminal to check your public IP address.
    ```bash
    curl ifconfig.me
    ```

2. **After Connecting to VPN:**
    - Connect to the Algo VPN using WireGuard.
    - Visit the same website or use the same `curl` command to check your public IP address again. It should now reflect the IP address of the VPN server, indicating that your traffic is being routed through the VPN.

### 2. Test Connectivity and Routing

#### Ping Tests
1. **Ping Internal Resources:**
    - If you have set up internal resources (e.g., servers or other devices on the VPN network), try pinging their IP addresses from the client machine.
    ```bash
    ping 10.19.49.1  # Example IP address of a resource within the VPN
    ```

2. **Ping External Resources:**
    - Ping external websites to ensure you have internet connectivity through the VPN.
    ```bash
    ping google.com
    ```

### 3. Test DNS Resolution
1. **Check DNS Resolution:**
    - Verify that DNS requests are being resolved correctly through the VPN. Use `nslookup` or `dig` to test DNS resolution.
    ```bash
    nslookup google.com
    ```
    - Ensure that the DNS server provided by Algo VPN (usually the VPN server itself or a specified DNS server) is being used.

### 4. Test Data Encryption
1. **Wireshark Packet Capture:**
    - Use Wireshark to capture and analyze network traffic.
    - Start Wireshark and capture packets on your network interface.
    - Look for WireGuard traffic (typically UDP) and ensure that data is encrypted.
    - Filter by protocol (e.g., `wg` for WireGuard or `udp.port == 51820` if you are using the default WireGuard port).

### 5. Test Firewall and Security
1. **Port Scanning:**
    - Use tools like `nmap` to perform port scanning from outside the VPN to ensure that your VPN server is not exposing unnecessary services.
    ```bash
    nmap -sS -O [your_vpn_server_ip]
    ```

2. **Security Audit:**
    - Check for open ports and services running on your VPN server.
    - Ensure that the firewall rules are correctly configured to block unauthorized access.

### 6. Test Performance
1. **Speed Test:**
    - Perform a speed test to check the performance of the VPN connection.
    - Use websites like [Speedtest](https://www.speedtest.net/) or command-line tools like `speedtest-cli`.
    ```bash
    speedtest-cli
    ```

### 7. Test Access to Restricted Content
1. **Access Geo-Restricted Services:**
    - Try accessing services or websites that are restricted based on geographic location to confirm that your IP address is recognized as being from the VPN server's location.

## Example Commands and Output

```bash
# Check public IP before VPN connection
curl ifconfig.me
# Output: Your public IP (e.g., 203.0.113.45)

# Check public IP after VPN connection
curl ifconfig.me
# Output: VPN server's public IP (e.g., 198.51.100.10)

# Ping internal resource within VPN
ping 10.19.49.1
# Output: PING 10.19.49.1 (10.19.49.1) 56(84) bytes of data.

# Ping external resource to check internet connectivity
ping google.com
# Output: PING google.com (172.217.11.46) 56(84) bytes of data.

# DNS resolution test
nslookup google.com
# Output: Server:  10.19.49.1
# Address: 10.19.49.1#53
# Non-authoritative answer:
# Name:    google.com
# Address: 172.217.11.46

# Speed test
speedtest-cli
# Output: Speed test results showing download/upload speeds through VPN
