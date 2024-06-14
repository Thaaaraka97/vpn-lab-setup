# OpenVPN Setup Instructions

## Prerequisites
- A Google Cloud Platform (GCP) account.
- A client device with OpenVPN installed.

## Installation

### 1. Set Up OpenVPN Server on Google Cloud Engine (GCE)

#### Step 1: Sign Up or Log In to Google Cloud Platform
1. Go to [Google Cloud Platform](https://cloud.google.com/).
2. Sign in with your Google account or create a new one.
3. Set up a new project or select an existing project.

#### Step 2: Access the Google Cloud Marketplace
1. Navigate to the [Google Cloud Marketplace](https://console.cloud.google.com/marketplace).
2. Search for "OpenVPN" in the search bar.
3. Select the "OpenVPN Access Server" from the results.

#### Step 3: Deploy the OpenVPN Access Server
1. Click the "Launch" button to begin the deployment process.
2. Configure the deployment settings:
    - Choose the desired zone and machine type (the default settings are usually sufficient for most use cases).
    - Set the instance name.
3. Click "Deploy" to start the deployment.

#### Step 4: Configure OpenVPN Access Server
1. Once the deployment is complete, navigate to the VM Instances page to find your OpenVPN server instance.
2. Note the external IP address of the instance.
3. Open a web browser and go to `https://<EXTERNAL_IP>:943/admin` to access the OpenVPN Access Server admin interface.
4. Log in using the default credentials:
    - Username: `openvpn`
    - Password: `your_instance_password` (set during the deployment process)

#### Step 5: Initial Configuration
1. Follow the on-screen instructions to complete the initial configuration.
2. Set up the admin account and password.
3. Configure the VPN settings as needed.

### 2. Configure OpenVPN Client

#### Install OpenVPN
- On Windows:
    - Download and install OpenVPN from [the official site](https://openvpn.net/community-downloads/).

- On macOS/Linux:
    ```bash
    brew install openvpn
    sudo apt-get install openvpn
    ```

#### Create Client Configuration File
1. **Download the Client Configuration File:**
    - Log in to the OpenVPN Access Server user portal at `https://<EXTERNAL_IP>:943/`.
    - Log in with the credentials you set during the initial configuration.
    - Download the client configuration file (e.g., `client.ovpn`).

2. **Transfer Configuration Files to Client Device:**
    - Copy `client.ovpn` to the client device.

#### Import Configuration to OpenVPN Client
- Open the OpenVPN client on your device and import the `client.ovpn` file.

### 3. Start the VPN
- Connect to the VPN using the OpenVPN client.

## Testing

### 1. Verify IP Address

#### Check Public IP Address
1. **Before Connecting to VPN:**
    - Open a terminal or command prompt and run:
    ```bash
    curl ifconfig.me
    ```
    - Note the IP address displayed.

2. **After Connecting to VPN:**
    - Connect to your VPN using OpenVPN Connect.
    - Run the same command again:
    ```bash
    curl ifconfig.me
    ```
    - The IP address should now be different, reflecting the IP address of the VPN server, indicating that your traffic is being routed through the VPN.

### 2. Test Connectivity and Routing

#### Ping Tests
1. **Ping External Websites:**
    - Open a terminal or command prompt and run:
    ```bash
    ping google.com
    ```
    - You should receive responses from Google, indicating that you have internet connectivity through the VPN.

2. **Ping Internal Resources:**
    - If you have set up internal resources within the VPN (e.g., other devices or servers), try pinging their IP addresses:
    ```bash
    ping 10.8.0.1  # Example IP address of a resource within the VPN
    ```
    - You should receive responses, indicating that you can communicate with other devices within the VPN network.

### 3. Test DNS Resolution
1. **Check DNS Resolution:**
    - Use `nslookup` or `dig` to verify that DNS requests are being resolved correctly through the VPN:
    ```bash
    nslookup google.com
    ```
    - Ensure that the DNS server provided by the VPN is being used. The DNS server IP address in the response should match the DNS settings configured for your VPN.

### 4. Test Data Encryption
1. **Wireshark Packet Capture:**
    - Use Wireshark to capture and analyze network traffic to ensure that data is encrypted.
    - Start Wireshark and capture packets on the network interface used by OpenVPN.
    - Look for OpenVPN traffic (typically using UDP or TCP on the port you configured, e.g., 1194).
    - Ensure that the data appears encrypted and cannot be easily read.

### 5. Test Firewall and Security
1. **Port Scanning:**
    - Use tools like `nmap` to perform port scanning from outside the VPN to ensure that your VPN server is not exposing unnecessary services:
    ```bash
    nmap -sS -O [your_vpn_server_ip]
    ```

2. **Security Audit:**
    - Check for open ports and services running on your VPN server.
    - Ensure that firewall rules are correctly configured to block unauthorized access.

### 6. Test Performance
1. **Speed Test:**
    - Perform a speed test to check the performance of the VPN connection:
    - Use websites like [Speedtest](https://www.speedtest.net/) or command-line tools like `speedtest-cli`:
    ```bash
    speedtest-cli
    ```
    - Compare the results to your regular internet connection speed to understand the impact of the VPN.

### 7. Test Access to Restricted Content
1. **Access Geo-Restricted Services:**
    - Try accessing services or websites that are restricted based on geographic location to confirm that your IP address is recognized as being from the VPN server's location.

## Example Commands and Outputs

```bash
# Check public IP before VPN connection
curl ifconfig.me
# Output: Your public IP (e.g., 203.0.113.45)

# Check public IP after VPN connection
curl ifconfig.me
# Output: VPN server's public IP (e.g., 198.51.100.10)

# Ping external website
ping google.com
# Output: PING google.com (172.217.11.46) 56(84) bytes of data.

# Ping internal resource within VPN
ping 10.8.0.1
# Output: PING 10.8.0.1 (10.8.0.1) 56(84) bytes of data.

# DNS resolution test
nslookup google.com
# Output: Server:  10.8.0.1
# Address: 10.8.0.1#53
# Non-authoritative answer:
# Name:    google.com
# Address: 172.217.11.46

# Speed test
speedtest-cli
# Output: Speed test results showing download/upload speeds through VPN
