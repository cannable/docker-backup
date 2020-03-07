# backup

Alpine with tar, for use with [https://docs.docker.com/storage/volumes/#backup-a-container](https://docs.docker.com/storage/volumes/#backup-a-container).

# Example

```bash
#!/bin/bash

# Variables
TARGET=pihole
OUTDIR=/backup
OUTTMP="${OUTDIR}/${TARGET}_backup.tgz.tmp"
OUTTAR="${OUTDIR}/${TARGET}_backup.tgz"
OLDTAR="${OUTDIR}/${TARGET}_backup.old.tgz"


# Perform live backup/tar of volumes
docker run --rm \
	--name backup_${TARGET} \
	--volumes-from ${TARGET} \
	-v "${OUTDIR}":"${OUTDIR}" \
	cannable/backup \
	tar czpf "${OUTTMP}" /etc/pihole/ /etc/dnsmasq.d/


# Rotate tar archives
mv $OUTTAR $OLDTAR
mv $OUTTMP $OUTTAR

sync
```

