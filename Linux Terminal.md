## Create Symlink
```bash
ln -s DESTINATION LINK
```
DESTINATION is the file/folder wich has data in it

LINK is the link that you will use to reach the DESTINATION

Example:
```bash
ln -s /mnt/tools/docker-compose-file/ /home/pi/compose-file
```
Here, if you access /home/pi/compose-file you will be inside /mnt/tools/docker-compose-file/


## Add User to Sudoers
```bash
usermod -aG sudo USERNAME
```


## Static IP on Debian


/etc/network/interfaces


```bash
# The primary network interface
auto enp0s5
iface enp0s5  inet static
 address 192.168.2.236
 netmask 255.255.255.0
 gateway 192.168.2.254
 dns-nameservers 192.168.2.254
```


## Extract File
```bash
tar -xzvf file.tar.gz
```
In a specific folder
```bash
tar -xzvf my.tar.gz -C /home/vivek/backups/
```

## Compress File
```bash
tar -czvf file.tar.gz /home/vivek/data/
```


## Copy file trought SSH
Syntax:

```bash
scp <source> <destination>
```

To copy a file from B to A while logged into B:
```bash
scp /path/to/file username@a:/path/to/destination
```

To copy a file from B to A while logged into A:
```bash
scp username@b:/path/to/file /path/to/destination
```

## Change Hostname
```bash
sudo nano /etc/hostname
```

## Create System User and Group with specific UID/GID and no login
```bash
sudo groupadd -g GID GROUPNAME
```

```bash
sudo adduser USERNAME --uid UID --gid GID --system
```

## Debian Upgrade
Upgrade Software
```bash
sudo apt update
```

```bash
sudo apt upgrade
```

```bash
sudo apt full-upgrade
```

Clean unused packages
```bash
sudo apt autoremove
```

Re-do (just to be sure)
```bash
sudo apt upgrade
```

Reboot to make changes effective

## Kernel Upgrade
Check kernerl version
```bash
uname -r
```

Search for kernel version available
```bash
sudo apt-cache search linux-image
```

Install new kernel
```bash
sudo apt install linux-image-<flavour>
```

Reboot to make changes effective

## Install WiFi driver
https://github.com/lwfinger/rtw88

## Disable Root Login
### Disable Shell Login
Open passwd file
```bash
sudo nano /etc/passwd
```
Change the line
```bash
root:x:0:0:root:/root:/bin/bash
```
to
```bash
root:x:0:0:root:/root:/sbin/nologin
```

### Disable SSH Login
Open SSH config file
```bash
sudo vim /etc/ssh/sshd_config
```

Uncomment the **PermitRootLogin** and set it to **no** or **prohibit-password**

## K8s Flag
You can list Kubernetes node details along with their labels in this fashion:
```bash
kubectl get nodes --show-labels
```

If you want to know the details for a specific node, use this:
```bash
kubectl label --list nodes node_name
```

Let's label that node with an appropriate name (like production):
```bash
kubectl label nodes <node-name> <label-name>=<label-value>
```

If you later decide to overwrite some labels based on the requirements see how you can achieve that.
```bash
kubectl label --overwrite nodes <node-name> <label-name>=<label-value>
```

To remove the label from a node, provide the key without any value.
```bash
kubectl label --overwrite nodes <node-name> <label-name>-
```
