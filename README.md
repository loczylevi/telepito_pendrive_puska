# telepito_pendrive_puska

## Természetes Ubuntu alatt mert a windows :triumph: :shit:

https://www.ventoy.net/en/index.html

Secure copy hint
```bash
scp source_username@192.168.1.1:/home/source_username/valami.txt /home/destination_username
```

__1. Ventoy letöltése__
```bash
cd ~/Letöltések
wget https://github.com/ventoy/Ventoy/releases/download/v1.0.97/ventoy-1.0.97-linux.tar.gz
```

__2. Ventoy kicsomagolása__

```bash
tar -xzf ventoy-*.tar.gz
cd ventoy-*
```

__/dev/sdb__ keress ilyen eszközt nevet feltehetöleg ez lesz a külsőmeghajtó

__lsblk__ --> kilistázza a particiokat i guess

```bash
lsblk
           ---kimenet---
sdc      8:32   1   7,5G  0 disk  
├─sdc1   8:33   1   7,4G  0 part /mnt
└─sdc2   8:34   1   32M   0 part 
```

__4. Ventoy telepítése a pendrive-ra legacy bios mod__
```bash
sudo ./Ventoy2Disk.sh -i /dev/sdb
```

__ISO fájlok másolása__

```bash
cp ~/Letöltések/ubuntu.iso /media/$USER/Ventoy/
cp ~/Letöltések/windows.iso /media/$USER/Ventoy/
```

__csatlakoztasd le a pendriveot (ha használods miközben irod az nem tul *barokkos*)__
```bash
sudo umount /dev/sdb1
```

## Átlagos probléma: szét van particionálva a pendrive nullázuk formázzuk nullára

```bash
sudo fdisk /dev/sdb
```

p – Megmutatja a partíciókat (ellenőrzéshez)
d – Partíció törlése (ha több van, ismételd meg)
w – Módosítások mentése és kilépés

__4. partició FAT32-vé formázása__

```bash
sudo mkfs.vfat -F32 /dev/sdb1
```


__csatlakoztasd vissza a pendriveot__

```bash
sudo mount /dev/sdb1 /mnt
```

__Menny oda ahova letöltötted és kibontottad a ventoyt!__
*sudo ./Ventoy2Disk.sh -i /dev/sdb* ezze la commandal legacy biosos ventoyt craftolsz

```bash
cd ~/Letöltések/ventoy-1.0.97
sudo ./Ventoy2Disk.sh -i /dev/sdb
```
:bulb: Figyelem!
NE írd be a partíció nevét (pl. /dev/sdb1), mert az egész lemezt kell Ventoy-nak kezelnie, nem csak egy partíciót.

# HA valami ilyesmit látsz akkor kész vagy :arrow_double_down: :arrow_down:
```bash
loczylevi@sis:~/Letöltések/ventoy-1.0.97$ sudo ./Ventoy2Disk.sh -I /dev/sdc

**********************************************
      Ventoy: 1.0.97  x86_64
      longpanda admin@ventoy.net
      https://www.ventoy.net
**********************************************

Disk : /dev/sdc
Model: VendorCo ProductCode (scsi)
Size : 7 GB
Style: MBR


Attention:
You will install Ventoy to /dev/sdc.
All the data on the disk /dev/sdc will be lost!!!

Continue? (y/n) y

All the data on the disk /dev/sdc will be lost!!!
Double-check. Continue? (y/n) y

Create partitions on /dev/sdc by parted in MBR style ...
Done
Wait for partitions ...
partition exist OK
create efi fat fs /dev/sdc2 ...
mkfs.fat 4.2 (2021-01-31)
success
Wait for partitions ...
/dev/sdc1 exist OK
/dev/sdc2 exist OK
partition exist OK
Format partition 1 /dev/sdc1 ...
mkexfatfs 1.3.0
Creating... done.
Flushing... done.
File system created successfully.
mkexfatfs success
writing data to disk ...
sync data ...
esp partition processing ...

Install Ventoy to /dev/sdc successfully finished.
```

bemásolod pendrivra az isokat oszt kész vagy


Hogyan telepítsd Ventoy-t Legacy BIOS-hoz?

```bash
sudo ./Ventoy2Disk.sh -I /dev/sdb
```

Hogyan lehet UEFI-re telepíteni?
Ha UEFI módban akarod telepíteni Ventoy-t (GPT partícióval), akkor ezt kell futtatni:

```bash
sudo ./Ventoy2Disk.sh -I -g /dev/sdb
```

