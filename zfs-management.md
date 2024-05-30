## ZFS create Pool
```bash
zpool create <pool-name> \
mirror <dev1> <dev2> \
mirror <dev3> <dev4>
```

```bash
zpool create <pool-name> \
raidz <dev1> <dev2> <dev3> \
raidz <dev4> <dev5>
```

## ZFS replace disk
```bash
zpool replace <pool> <disk-to-remove> <new-disk>
```
