# CTF Challenge - Exploiting GetSimple CMS

This repository contains the steps and notes I took while completing a Capture The Flag (CTF) challenge on Hack The Box. In this CTF, I exploited a vulnerability in a GetSimple CMS installation to gain a reverse shell and escalate privileges to root.

## Overview

This CTF challenge involved leveraging weak credentials in the admin panel of a GetSimple CMS installation to gain initial access to the target. After successfully logging in, I executed a reverse shell by editing a PHP template file. Finally, I used `sudo` privileges to escalate to root access.

## Steps to Exploit

### 1. Initial Enumeration
I started by connecting to the Hack The Box VPN using OpenVPN and enumerated the target using `nmap`. The target had two open ports:
- **22** (SSH)
- **80** (HTTP)

After inspecting the HTTP service, I found it was running **GetSimple CMS**.

### 2. HTTP Enumeration
I discovered the `/admin/` directory through a robots.txt file and used **gobuster** to find hidden directories like `/backups/` and `/data/`.

### 3. Admin Login Exploit
By inspecting the `/data/` directory, I found a `users.xml` file that contained credentials for the admin user. Although the password in the file didn’t work, I successfully guessed the password as `admin` for the `admin` user.

### 4. Reverse Shell
After logging into the admin panel, I modified the PHP theme template to execute a reverse shell, which connected back to my machine on port 9443. Using **netcat**, I established a foothold on the target machine.

### 5. Privilege Escalation
Upon gaining access to the target, I ran `sudo -l` and discovered that I could run PHP with root privileges. I used this to escalate to a root shell and retrieve the root flag.

## File Structure

The repository is structured as follows:

├── scans # All of my enumeration scans
│  ├── gobuster_scan
│  ├── initial_scan.gnmap 
│  ├── initial_scan.nmap
│  ├── initial_scan.xml
│  ├── nmap_http_enum.gnmap
│  ├── nmap_http_enum.nmap
│  ├── nmap_http_enum.xml
│  ├── script_scan.gnmap
│  ├── script_scan.nmap
│  ├── script_scan.xml
├── screenshots # Images of important discoveries
│  ├── admin_page.png
│  ├── exploiting_admin_page.png
│  ├── reverse_shell.png
│  ├── test_php_command.png
├── notes.txt # Some important commands and outputs

## Conclusion
This was a great learning experience that reinforced the importance of proper enumeration and privilege escalation techniques.