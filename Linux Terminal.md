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
