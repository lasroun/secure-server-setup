# Change the Default Root User to a New User on a Linux Server

## Objective
A simple guide to add a new admin user and disable direct root login via SSH for enhanced server security.

---

## Prerequisites
- Root access to your server
- Active SSH connection

---

## Steps

### 1. Create a New User
```bash
adduser newuser
```
Replace `newuser` with the desired username.

Follow the prompts to set a password and fill in the optional fields.

### 2. Add the User to the Sudo Group
```bash
usermod -aG sudo newuser
```
This grants the user administrative privileges.

### 3. Test User Privileges
```bash
su - newuser
sudo ls /root
```
Ensure the user can execute commands with sudo.

### 4. Configure SSH to Disable Root Access
Edit the SSH configuration:
```bash
sudo nano /etc/ssh/sshd_config
```

Find the line:
```bash
PermitRootLogin yes
```
Replace it with:
```bash
PermitRootLogin no
```

### 5. Restart SSH
```bash
sudo systemctl restart sshd
```

### 6. Test the Connection
Open a new SSH session and connect with:
```bash
ssh newuser@server_ip
```

---

## Revert to Root in Case of Issues
If necessary, you can always revert to root using the direct console (e.g., through your provider's control panel).

```bash
sudo -i
```

---