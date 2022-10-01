## Create Symlink
```
ln -s DESTINATION LINK
```
DESTINATION is the file/folder wich has data in it

LINK is the link that you will use to reach the DESTINATION

Example:
```
ln -s /mnt/tools/docker-compose-file/ /home/pi/compose-file
```
Here, if you access /home/pi/compose-file you will be inside /mnt/tools/docker-compose-file/


## Add User to Sudoers
```
usermod -aG sudo USERNAME
```


## Static IP on Debian


/etc/network/interfaces


```
# The primary network interface
auto enp0s5
iface enp0s5  inet static
 address 192.168.2.236
 netmask 255.255.255.0
 gateway 192.168.2.254
 dns-nameservers 192.168.2.254
```
