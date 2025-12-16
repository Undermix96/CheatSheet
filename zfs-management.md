# Best Practices ZFS 2025 (Debian 13 / OpenZFS)

Nel 2025, la gestione di ZFS su Linux si è evoluta per massimizzare le prestazioni degli SSD NVMe e la densità degli HDD. Di seguito le linee guida fondamentali.

## 1. Configurazione del Pool (`zpool`)

*   **Identificazione Dischi:** Usa sempre `/dev/disk/by-id/`. Evita `/dev/sdX` poiché i nomi possono cambiare al riavvio, causando il mancato montaggio del pool.
*   **Allineamento Settori (`ashift`):** 
    *   Usa sempre `-o ashift=12` per dischi con settori da 4K (standard attuale).
    *   Per alcuni SSD NVMe moderni, valuta `ashift=13` (settori da 8K).
*   **No RAID Hardware:** ZFS deve comunicare direttamente con i dischi. Usa controller in modalità **IT (HBA)**, mai RAID hardware.
*   **Ridondanza:**
    *   **Mirror:** Ideale per prestazioni e velocità di resilver.
    *   **RAID-Z2:** Scelta consigliata per storage ad alta densità (sicurezza contro la rottura di 2 dischi). Evita RAID-Z1 per dischi superiori a 2TB.

## 2. Configurazione dei Dataset (`zfs create`)

Queste proprietà dovrebbero essere impostate a livello di pool o di dataset principale per essere ereditate:

*   **Compressione:**
    *   `compression=lz4`: Il default per quasi tutto (estremamente veloce).
    *   `compression=zstd`: Da usare per dati d'archivio o documentazione (ottimo rapporto spazio/CPU).
*   **Prestazioni Linux:**
    *   `xattr=sa`: Fondamentale su Linux per memorizzare gli attributi estesi in modo efficiente.
    *   `atime=off`: Disabilita l'aggiornamento dell'orario di accesso per ridurre le scritture non necessarie.
*   **Ottimizzazione `recordsize`:**
    *   **Default (128k):** Ottimo per uso generico.
    *   **Database (16k):** Per MySQL o PostgreSQL (riduce il write amplification).
    *   **Media (1M):** Per grandi file video (migliora lo streaming e riduce i metadati).

## 3. Manutenzione, Sicurezza e Gestione dello Spazio

*   **Soglia di riempimento:** Mantieni l'occupazione del pool sotto l'**80%**. Oltre questa soglia, ZFS passa da un'allocazione "first-fit" a una "best-fit", rallentando drasticamente.
*   **ZFS Scrub:** Pianifica uno scrub mensile via `cron` o `systemd timer` per rilevare e correggere errori silenti dei dati (bit-rot).
*   **Special VDEV:** Se hai un pool di HDD, l'aggiunta di un piccolo mirror di SSD come `special` device per i metadati trasformerà la reattività del sistema.
*   **Encryption:** Se hai bisogno di crittografia, usa quella nativa di ZFS in fase di creazione del dataset. Nel 2025 è stabile e performante grazie alle istruzioni AES-NI delle CPU moderne. 

---

## Esempio di creazione Pool Ottimizzato (2025)

```bash
sudo zpool create -o ashift=12 \
    -O compression=lz4 \
    -O xattr=sa \
    -O atime=off \
    -O mountpoint=/mnt/data \
    nome_pool mirror \
    /dev/disk/by-id/ata-ID_DISCO_1 \
    /dev/disk/by-id/ata-ID_DISCO_2
```



# Comandi Vari

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

## Get Properties
```bash
zfs get all pool/dataset
```

