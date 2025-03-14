 # MISP + Pi-hole Installation Guide
 
 This README provides step-by-step instructions for installing both **MISP** (Malware Information Sharing Platform) and **Pi-hole** on a Debian-based system (tested on Debian 12). The process uses the official installation scripts from each project’s GitHub repository.
 
 ---
 
 ## Important
 
 - These scripts assume you have **sudo** or **root** privileges.  
 - MISP and Pi-hole each have their own dependencies; ensure your system is up-to-date before proceeding.  
 - If you plan to run both on the **same machine**, verify that there are no port conflicts (e.g., Pi-hole typically runs a DNS service and a web dashboard).  
 - Consult each project’s official documentation for more advanced configuration.
 
 ---
 
 ## Prerequisites
 
 ### Operating System
 - Debian 12 or later (other Debian-based distributions might work but are not officially tested).
 
 ### Updated Packages
 ```bash
 sudo apt-get update && sudo apt-get upgrade -y
 ```
 
 ### Basic Tools
 Install `curl`, `git`, `bash`, etc. if missing:
 ```bash
 sudo apt-get install curl git -y
 ```
 
 ---
 
 ## 1. Install MISP
 
 ### 1.1 Download and Review the Script
 
 - **Option A**: Clone the MISP repository:
   ```bash
   git clone https://github.com/MISP/MISP.git
   ```
   The installer script is located at:
   ```swift
   MISP/INSTALL/INSTALL.debian12.sh
   ```
 
 - **Option B**: Directly download the script:
   ```bash
   curl -O https://raw.githubusercontent.com/MISP/MISP/2.5/INSTALL/INSTALL.debian12.sh
   ```
 
 - Make the script executable:
   ```bash
   chmod +x INSTALL.debian12.sh
   ```
 
 ### 1.2 Run the MISP Installation Script
 ```bash
 sudo ./INSTALL.debian12.sh
 ```
 - Follow the on-screen prompts.  
 - The script will install dependencies, configure Apache/MySQL, and set up MISP’s web interface by default.  
 - When finished, confirm that MISP is running by opening your server’s IP/hostname in a web browser (default is `http://server-ip/`). You should see the MISP login page.
 
 ### 1.3 Post-Installation Tasks
 - **Default Credentials**: Refer to the script’s output or the [MISP documentation](https://github.com/MISP/MISP) for default login details.  
 - **Configuration**: Fine-tune settings (e.g., email notifications, security keys) in `app/Config` inside the MISP directory.  
 - **Updates**: Keep your MISP instance updated by pulling the latest changes from GitHub and following MISP’s official upgrade procedure.
 
 ---
 
 ## 2. Install Pi-hole
 
 ### 2.1 Download the Installer
 There are multiple ways to download Pi-hole, but the official recommendation is to run their `basic-install.sh` script. For example:
 ```bash
 curl -sSL https://install.pi-hole.net | bash
 ```
 
 Alternatively, you can manually download `basic-install.sh` from the Pi-hole repo:
 ```bash
 curl -O https://raw.githubusercontent.com/pi-hole/pi-hole/master/automated%20install/basic-install.sh
 chmod +x basic-install.sh
 sudo ./basic-install.sh
 ```
 
 ### 2.2 Follow the Interactive Prompts
 - The script will ask for DNS provider preferences, IP configuration, and other optional settings.  
 - You can customize these details to match your network requirements.
 
 ### 2.3 Verify Pi-hole Status
 - After installation, Pi-hole’s admin interface is typically accessible at `http://<IP_ADDRESS>/admin`.  
 - To confirm it is serving DNS queries, set your computer’s DNS server to the IP of the Pi-hole machine and test browsing functionality.
 
 ---
 
 ## 3. Considerations for Running MISP and Pi-hole Together
 
 ### Port Conflicts
 - MISP defaults to serving via **HTTP/HTTPS** (ports 80/443) under Apache.  
 - Pi-hole also hosts a web interface on port 80 (or 443 if SSL is configured).  
 - If both are on the same server, you may need to reconfigure one of the web services to avoid conflicts (e.g., move MISP or Pi-hole to a non-standard port).
 
 ### System Resources
 - MISP can be resource-intensive if you enable frequent updates or many event feeds.  
 - Pi-hole is relatively lightweight but demands consistent DNS availability.  
 - On a low-spec device, monitor CPU/memory usage to ensure performance remains acceptable.
 
 ### Security
 - Regularly update both MISP and Pi-hole to patch known vulnerabilities.  
 - Consider using a firewall or intrusion detection system if exposing MISP externally.  
 - Restrict access to Pi-hole’s admin dashboard to trusted IP ranges.
 
 ---
 
 ## 4. Troubleshooting
 
 - **MISP Not Reachable**: Check Apache logs (`/var/log/apache2/`) for errors, confirm MySQL is running, and ensure the firewall allows inbound HTTP/HTTPS traffic.  
 - **DNS Resolution Issues**: If Pi-hole is not resolving queries, confirm your network settings (DNS is set to Pi-hole’s IP) and review `/var/log/pihole.log` for clues.  
 - **Updates**: If installation or updates fail, re-run scripts in a clean environment or consult each project’s GitHub issues/discussion forum.
 
 ---
 
 ## 5. References & Further Reading
 
 - **MISP Documentation**:  
   [https://github.com/MISP/MISP](https://github.com/MISP/MISP)
 - **Pi-hole Documentation**:  
   [https://github.com/pi-hole/pi-hole](https://github.com/pi-hole/pi-hole)
 - **Official MISP Debian 12 Install Script**:  
   [https://github.com/MISP/MISP/blob/2.5/INSTALL/INSTALL.debian12.sh](https://github.com/MISP/MISP/blob/2.5/INSTALL/INSTALL.debian12.sh)
 - **Pi-hole Basic Install Script**:  
   [https://github.com/pi-hole/pi-hole/blob/master/automated%20install/basic-install.sh](https://github.com/pi-hole/pi-hole/blob/master/automated%20install/basic-install.sh)
 ```
