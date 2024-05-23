### Backup DB
```bash
pg_dump -f <file-to-create> -F tar -h <db-ip> -p <db-port> -U <username> -d <db-name> --verbose
```


### Restore DB
```bash
pg_restore -h <db-ip> -p <db-port> -U <username> -d <db-name> --verbose <file-to-restore>
```