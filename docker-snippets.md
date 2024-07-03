
### Manage docker from inside a container
```bash
docker run --rm \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /usr/bin/docker:/usr/bin/docker \
-v /usr/lib/libdevmapper.so.1.02:/usr/lib/libdevmapper.so.1.02 \
ubuntu docker --version
```
