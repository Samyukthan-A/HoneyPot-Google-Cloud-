# HoneyPot-Google-Cloud


![cover](https://github.com/user-attachments/assets/c860b6d5-bc2a-4423-af8b-a289a2c8287f)

A 24-hour analysis of cyber threats using T-Pot on Google Cloud, capturing ~65,000 attacks. This project provides insights into attack patterns, sources, and vulnerabilities, offering a foundation for enhancing cybersecurity strategies.

## Project Overview

This repository documents the deployment and analysis of a T-Pot honeypot system on Google Cloud. The system ran on **Ubuntu 24.04 LTS** with **15 GB RAM** and **4 vCPUs**, and was active for 24 hours starting from **June 13, 2025, 07:04 PM IST**. During this period, it captured approximately **65,000 attacks**. The analysis covers attack sources, targeted ports, Suricata alerts, CVEs, and credential attempts, providing a comprehensive view of the threat landscape.

## System Configuration

- **Platform**: Google Cloud
- **Operating System**: Ubuntu 24.04 LTS
- **Region Deployed**: us-west1-c
- **Honeypot Software**: T-Pot (Hive installation)
- **Hardware Specifications**:
  - **Machine Type**: n1-standard-4
  - **RAM**: 15 GB
  - **vCPUs**: 4
  - **CPU Platform**: Intel Broadwell x86/64
- **Ports Allowed**: All (configured to allow all inbound traffic for honeypot services; administrative ports like SSH should be secured)
- **Deployment Duration**: 24 hours
- **Total Attacks Recorded**: ~65,000

## Setup Instructions

Follow these steps to deploy T-Pot on a Google Cloud instance. This guide assumes you are starting with a fresh instance and have basic familiarity with Google Cloud and Linux.

1. **Create a Google Cloud Instance**:
   - Launch a new VM instance in Google Cloud.
   - Select **Ubuntu 24.04 LTS** as the operating system (T-Pot also supports other OSes like Debian; check the [T-Pot GitHub](https://github.com/telekom-security/tpotce) for compatibility).
   - Choose the **n1-standard-4** machine type (15 GB RAM, 4 vCPUs) to match this deployment.
   - Set the region to **us-west1-c** (or your preferred region).
   - Configure the firewall to allow all inbound traffic for honeypot services (see step 9 for details).

2. **Install Prerequisites**:
   - Connect to your instance via SSH.
   - Update the package lists and install Git:
     ```bash
     sudo apt update && sudo apt upgrade -y
     sudo apt install git -y
     ```

3. **Clone the T-Pot Repository**:
   - Clone the T-Pot repository from GitHub:
     ```bash
     git clone https://github.com/telekom-security/tpotce
     ```

4. **Navigate to the T-Pot Directory**:
   - Change into the `tpotce` folder:
     ```bash
     cd tpotce
     ```

5. **Run the Installer**:
   - Execute the installation script as a non-root user (do not run as root, as the script will prompt for `sudo` when needed):
     ```bash
     ./install.sh
     ```

6. **Choose the T-Pot Installation Type**:
   - When prompted, select the installation type. The options are:
     - **(H)ive**: Full-featured installation (used in this project, requires 8 GB+ RAM).
     - **(S)ensor**: Lightweight sensor for distributed setups (4 GB+ RAM).
     - **(L)LM**: LLM-based honeypots (requires OLLama or ChatGPT).
     - **(M)ini**: Minimal installation with 30+ honeypots (4 GB+ RAM).
     - **(M)obile**: For T-Pot Mobile deployments (available separately).
     - **(T)arpit**: Tarpit-focused installation.
   - For this project, select **(H)ive**:
     ```
     Choose your T-Pot type: H
     ```

7. **Configure the Web UI**:
   - The installer will prompt you to set a username and password for the T-Pot Web UI (e.g., Cockpit or EWBF for visualization).
   - Provide a secure username and password, and note them down for later use.

8. **Reboot the System**:
   - After the installation completes, reboot the instance to ensure all services start correctly:
     ```bash
     sudo reboot
     ```

9. **Configure Google Cloud Firewall**:
   - In the Google Cloud Console, navigate to **VPC Network > Firewall**.
   - Create a new firewall rule to allow inbound traffic for T-Pot’s honeypot services:
     - **Target**: Apply to your instance (e.g., by tag or all instances in the network).
     - **Source IP Ranges**: `0.0.0.0/0` (to allow all traffic, as in this project; for production, restrict to specific ports).
     - **Protocols and Ports**: Allow all (or specify ports like 22, 21, 23, 80, 443, etc., based on your honeypots; see T-Pot documentation for port details).
     - **Secure Administrative Access**: Ensure SSH (port 22) and Web UI ports (e.g., 64297 for Cockpit) are accessible only from trusted IPs, or use Google Cloud’s IAP (Identity-Aware Proxy) for secure access.
   - Note: Allowing all ports, as in this project, maximizes attack capture but increases risk; secure administrative access separately.

10. **Access the Web UI**:
    - After reboot, access the T-Pot Web UI:
      - Open a browser and navigate to `https://<instance-ip>:64297` (default port for Cockpit; adjust if different).
      - Log in using the username and password set during installation.
    - The Web UI provides dashboards for monitoring attacks, viewing logs, and analyzing data (e.g., via Kibana or EWBF).

### Additional Setup Notes
- **Prerequisites**: Ensure your system meets T-Pot’s requirements:
  - Docker, Docker Compose, and other dependencies are automatically installed by the `install.sh` script.
  - Minimum 8 GB RAM for the Hive installation (your setup with 15 GB exceeds this).
- **Security Considerations**:
  - Restrict administrative access (SSH, Web UI) to specific IP ranges or use a VPN.
  - Regularly update the system and T-Pot software to mitigate vulnerabilities.
- **Troubleshooting**:
  - If the Web UI is inaccessible, check if the service is running (`sudo systemctl status cockpit`) and verify firewall rules.
  - Review the T-Pot logs in `/opt/tpot/log/` for installation or runtime errors.

For detailed setup instructions and advanced configurations, refer to the official [T-Pot GitHub repository](https://github.com/telekom-security/tpotce).

## Next Steps
Once T-Pot is running, you can:
- Monitor incoming attacks via the Web UI.
- Analyze logs and visualizations (e.g., Kibana for Suricata alerts, tag clouds).
- Use the captured data (as in this project) to identify attack patterns and improve your security posture.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments
- [Telekom Security](https://github.com/telekom-security) for developing T-Pot.
- The open-source cybersecurity community for their contributions and tools.
