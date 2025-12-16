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

## Various

Get all Properties
```bash
zfs get all pool/dataset
```

## Best practices 2025

Usa /dev/disk/by-id/

Usa ashift=12

Dataset
Usa compression=lz4

Ottimizzazione recordsize:
Dati generici: Lascia il default di 128k.
Database (MySQL/PostgreSQL): Impostalo a 16k o al valore della pagina del DB per evitare la frammentazione e migliorare le prestazioni.
Media/Video: Usa 1M o superiore per ridurre i metadati e migliorare le letture sequenziali.

Usa atime=off

Usa Encryption
