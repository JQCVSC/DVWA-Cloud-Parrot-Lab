# DVWA-Cloud-Parrot-Lab

This project provides a guide for setting up Damn Vulnerable Web Application (DVWA) on a cloud VM and testing it using Parrot OS running locally.

**Note:** This project is not affiliated with or endorsed by the original DVWA project.

## For the official DVWA repository, please visit [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)

![image](https://github.com/user-attachments/assets/bf1b72ce-687b-45b5-aa93-38de749a223c)

## Overview

1. Cloud VM: Hosts DVWA, accessible only through SSH tunneling
2. Local Machine: Runs Parrot OS in a virtual machine for penetration testing

This setup allows for a secure, isolated environment to practice ethical hacking techniques.

## Key Components

1. Cloud VM (Google Cloud or AWS) running DVWA
2. Local VM running Parrot OS
3. SSH tunneling for secure access to DVWA

## Benefits

- DVWA is isolated in the cloud, reducing risk to your local network
- Parrot OS provides a full suite of penetration testing tools
- SSH tunneling ensures secure access to the vulnerable application

## Table of Contents

1. [Cloud VM Setup](#cloud-vm-setup)
2. [DVWA Installation](#dvwa-installation)
3. [Accessing DVWA](#accessing-dvwa)
4. [Using Parrot OS for Testing](#using-parrot-os-for-testing)
5. [Vulnerability Testing Guide](#vulnerability-testing-guide)
6. [Security Considerations](#security-considerations)
7. [Disclaimer](#disclaimer)

## Cloud VM Setup

![image](https://github.com/user-attachments/assets/57a696a2-8b38-492f-a9cf-017fea167bdc)

1. Create a VM instance on AWS EC2 or Google Cloud Platform.
2. Choose a Ubuntu 20.04 LTS image.
3. Configure security groups/firewall rules to allow SSH access only.
4. Generate and download SSH keys.

## DVWA Installation

1. SSH into your VM:
   
   Copy
```gcloud compute ssh dvwa-vm --zone=your-zone  # For GCP
ssh -i your-key.pem ubuntu@your-instance-ip  # For AWS
```

2. Update system and install dependencies:

   Copy
```sudo apt update
sudo apt install -y apache2 php php-mysqli php-gd libapache2-mod-php git mysql-server
```

3. Clone DVWA:

   Copy
```
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
```

4. Configure DVWA: Update database settings, ensure 'db_server' is set to '127.0.0.1'.

   Copy
```
sudo cp /var/www/html/DVWA/config/config.inc.php.dist /var/www/html/DVWA/config/config.inc.php
sudo nano /var/www/html/DVWA/config/config.inc.php
```


5. Set up MySQL:

   Copy
```
sudo mysql
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

6. Configure Apache:

  Copy
```  
sudo nano /etc/apache2/sites-available/000-default.conf
Change DocumentRoot to `/var/www/html/DVWA`
```

7. Restart Apache:

   Copy
```
sudo systemctl restart apache2
```

## Accessing DVWA

1. Set up an SSH tunnel:
gcloud compute ssh dvwa-vm --zone=your-zone -- -L 8080:localhost:80  # For GCP
ssh -i your-key.pem -L 8080:localhost:80 ubuntu@your-instance-ip  # For AWS
Copy
2. Access DVWA through your browser at `http://localhost:8080`

3. Click on start/reset databsae at the bottome of the webpage

4. Log in with default credentials: admin / password

![image](https://github.com/user-attachments/assets/1616a4de-afb4-4e61-88fb-58679628f1ab)

## Using Parrot OS for Testing

![image](https://github.com/user-attachments/assets/feb3ef58-f1f5-4024-8be6-3af9862a7f01)

1. Download and install Parrot OS Security Edition as a VM.
2. Ensure Parrot OS VM and your cloud VM are on isolated networks.
3. Use Parrot's built-in tools for testing DVWA vulnerabilities.

## Vulnerability Testing Guide

Parrot OS includes tools for testing each of DVWA's vulnerabilities:

1. Brute Force: Use Hydra or Medusa
2. Command Injection: Manual testing and custom scripts
3. CSRF: Burp Suite's CSRF PoC generator
4. File Inclusion: Manual testing and custom scripts
5. File Upload: Upload potential malicious files (non-executable in this context)
6. Insecure CAPTCHA: Manual testing
7. SQL Injection: SQLmap
8. SQL Injection (Blind): SQLmap with --blind flag
9. Weak Session IDs: Burp Suite Sequencer
10. XSS (DOM, Reflected, Stored): XSSer and manual testing
11. CSP Bypass: Manual testing and CSP Evaluator
12. JavaScript: Browser's Developer Tools
13. Authorization Bypass: Manual testing and Burp Suite
14. Open HTTP Redirect: Manual testing and OWASP ZAP

## Security Considerations

- Keep the VM firewalled and accessible only through SSH.
- Use strong, unique passwords for all services.
- Regularly update the VM's operating system and installed software.
- Never use this setup for testing systems you don't own or have explicit permission to test.

## Disclaimer

This setup is for educational purposes only. Never use these techniques or tools against systems you do not own or have explicit permission to test. The user assumes all responsibility for the use of this information.
