# Change the Default Root User to a New User on a Linux Server

## Objective

Create a new user with sudo privileges, configure SSH access, and disable direct root access to enhance your server's security.

---

## Prerequisites

- Root access to your server
- Active SSH connection

---

## Steps

### 1. Connect to the Server via SSH as Root

Retrieve the IP address of your server from the Digital Ocean interface (or your server provider's dashboard).

Use the following command to connect as root:

```bash
ssh root@server_ip
```

Example:

```bash
ssh root@127.0.0.1
```

If this is your first connection, you will be prompted to confirm the authenticity of the host. Type `yes` to proceed.

---

### 2. Create a New User

```bash
adduser newuser
```

Replace `newuser` with the desired username.

Follow the prompts to set a password and fill in the optional fields.

---

### 3. Grant Sudo Privileges to the New User

```bash
gpasswd -a newuser sudo
```

This adds the user to the sudo group, allowing administrative commands to be executed.

---

### 4. Configure SSH for the New User

Switch to the new user:

```bash
su - newuser
```

Create the `.ssh` directory and set appropriate permissions:

```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
```

Create the `authorized_keys` file to store your SSH key:

```bash
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Now, open the `authorized_keys` file with nano to paste your public SSH key:

```bash
nano ~/.ssh/authorized_keys
```

On your **local machine**, display your public SSH key by running:

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the output and paste it into the nano editor on the server. Save and exit nano by pressing `CTRL + O`, then `Enter`, and `CTRL + X`.

(Optionnal) : set ssh key

```
ssh-keygen -t ed25519 -C "your-email@example.com"
```

---

### 5. Disable Root SSH Access

Return to the root session:

```bash
exit
```

Edit the SSH configuration file:

```bash
nano /etc/ssh/sshd_config
```

Locate the following line:

```bash
PermitRootLogin yes
```

Change it to:

```bash
PermitRootLogin no
```

Save and exit the editor, then reload SSH to apply changes:

```bash
service ssh reload
```

---

### 6. Test the Connection

Open a new terminal and try connecting with the new user:

```bash
ssh newuser@server_ip
```

Ensure the connection works before closing your current session.

---

## Revert to Root in Case of Issues

If necessary, access the server through the provider's console and log in as root:

```bash
sudo -i
```

---
