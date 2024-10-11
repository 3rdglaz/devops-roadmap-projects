# devops-roadmap-projects
Hosts Projects from https://roadmap.sh/projects/ssh-remote-server-setup

Requirements:
1. An remote host
2. OpenSSH-server installed on remote host

## Create Public Keys

```sh
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_1st_key
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_2nd_key
```

## Update it to remote host

```sh
ssh-copy-id -i ~/.ssh/id_rsa_1st_key.pub user@server
ssh-copy-id -i ~/.ssh/id_rsa_2nd_key.pub user@server
```

## Check access with keys

```
ssh -i id_rsa_1st_key.pub user@server
```

## Change SSH Config to allow only access with keys

```sh
sudo nano /etc/ssh/sshd_config
```
Changer to match below

```sh
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

## Restart SSH Service on Server(Remote Host)

```sh
sudo systemctl restart ssh
```

## Allow simple connection

to use `ssh connection` on local machine create and edit `~/.ssh/config`

```sh
# Config for first key
Host connect
  HostName server
  User user
  IdentityFile ~/.ssh/id_rsa_1st_key
```

---

## To configure Fail2Ban 

1. **Install Fail2Ban**:

   ```bash
   sudo apt update
   sudo apt install fail2ban
   ```

2. **Configure Fail2Ban**:

   - Copy the default config:
     ```bash
     sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
     ```

   - Edit the config to enable SSH protection:
     ```bash
     sudo vim /etc/fail2ban/jail.local
     ```

     In the `[sshd]` section, ensure it looks like this:

     ```ini
     [sshd]
     enabled = true
     maxretry = 5
     bantime = 10m
     findtime = 10m
     ```

4. **Start Fail2Ban**:

   ```bash
   sudo systemctl start fail2ban
   sudo systemctl enable fail2ban
   ```

6. **Check Fail2Ban Status**:

   ```bash
   sudo fail2ban-client status sshd
   ```

