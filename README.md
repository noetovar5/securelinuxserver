# securelinuxserver
step by step guide to secure linux server on the network


 #A comprehensive guide on securing an Ubuntu server on a network and setting up automatic updates using cronjobs.

### Securing an Ubuntu Server on the Network

#### Step 1: Update and Upgrade
Ensure your server is up-to-date before implementing security measures.
```bash
sudo apt update
sudo apt upgrade
```

#### Step 2: Secure SSH Access
1. Change default SSH port (optional):
   Edit `/etc/ssh/sshd_config` and modify the line with `Port` to a non-standard port (e.g., 2222).
2. Disable root login:
   Set `PermitRootLogin no` in `/etc/ssh/sshd_config`.
3. Use SSH keys for authentication:
   Generate keys on your local machine and copy the public key to the server's `~/.ssh/authorized_keys` file.

#### Step 3: Firewall Configuration (using UFW)
Install Uncomplicated Firewall (UFW):
```bash
sudo apt install ufw
```
Configure UFW:
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow <SSH_PORT>/tcp
sudo ufw enable
```

#### Step 4: Install and Configure Fail2ban
Install Fail2ban:
```bash
sudo apt install fail2ban
```
Configure Fail2ban:
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
# Configure desired settings, e.g., bantime, findtime, maxretry.
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

#### Step 5: Harden System Security
1. Disable unused services.
2. Regularly update software.
3. Install and configure intrusion detection systems like Tripwire or AIDE.
4. Enable automatic security updates.

### Setting Up Automatic Updates Using Cronjob

#### Step 1: Open CronTab
```bash
crontab -e
```

#### Step 2: Add Cronjob to Run Updates at 2 AM
Add the following line to run updates daily at 2 AM:
```bash
0 2 * * * /usr/bin/apt update && /usr/bin/apt -y upgrade
```

#### Step 3: Save and Exit
If you're using nano editor, press `Ctrl + X`, then confirm changes and exit.

#### Step 4: Verify Cronjob
List the crontab to confirm the addition:
```bash
crontab -l
```

#### Step 5: Check Automatic Updates
Ensure that the updates run as expected by checking logs or monitoring the system after 2 AM.

Remember, security is an ongoing process. Regularly review logs, apply security patches, and stay informed about emerging threats to keep your server secure.
