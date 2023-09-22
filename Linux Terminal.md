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
sudo apt-get update
```

```bash
sudo apt-get upgrade
```

```bash
sudo apt-get full-upgrade
```

Clean unused packages
```bash
sudo apt-get autoremove
```

Re-do (just to be sure)
```bash
sudo apt-get upgrade
```

Reboot to make changes effective
