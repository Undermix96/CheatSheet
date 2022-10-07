### Static IP

First edit the file:
```bash
sudo nano /etc/dhcpcd.conf
```

Insert in the file these lines:
```bash
# Example static IP configuration:
interface eth0
static ip_address=192.168.1.152/24
#static ip6_address=fd51:42f8:caae:d92e::ff/64
static routers=192.168.1.1
static domain_name_servers=1.1.1.1 8.8.8.8
```
