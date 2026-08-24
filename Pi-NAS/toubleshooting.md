# NAS Troubleshooting

This document records the main issues encountered while building the Raspberry Pi NAS and how they were resolved.

## NVMe Not Detected

### Problem

The NVMe SSD initially did not appear in `lsblk`.

```text
mmcblk0
└── mmcblk0p2
```

The expected `nvme0n1` device was missing.

### Investigation

PCIe devices were checked:

```bash
lspci -nn
```

The Raspberry Pi 5 PCIe bridge and RP1 South Bridge were detected, but no NVMe controller appeared.

NVMe detection was also checked:

```bash
sudo dmesg | grep -i nvme
```

PCIe kernel messages were then examined:

```bash
sudo dmesg | grep -iE "pcie|pci"
```

The PCIe link was successfully established, indicating that the Raspberry Pi's PCIe subsystem was functioning.

### Resolution

The physical connection between the Raspberry Pi 5, PCIe FFC cable, X1001 NVMe HAT, and SSD was checked and reseated.

The NVMe was eventually detected successfully:

```text
nvme0n1
└── nvme0n1p1
```

The SSD was then partitioned and formatted with EXT4.

---

## Bootloader Update

The Raspberry Pi reported that a newer bootloader version was available.

The bootloader was updated with:

```bash
sudo rpi-eeprom-update -a
```

The Raspberry Pi was then rebooted and the NVMe detection was tested again.

---

## Persistent Mount Warning

After modifying `/etc/fstab`, the system reported:

```text
fstab has been modified, but systemd still uses
the old version
```

This was resolved by reloading systemd:

```bash
sudo systemctl daemon-reload
```

The mount was then tested:

```bash
sudo umount /mnt/nas
sudo mount -a
```

The NVMe successfully mounted at:

```text
/mnt/nas
```

---

## NAS Verification

The final storage configuration was verified with:

```bash
lsblk
```

The NVMe appeared as:

```text
nvme0n1
└── nvme0n1p1  /mnt/nas
```

The NAS was then configured with Samba and successfully tested from Windows.

File read and write operations were confirmed to work through the network share.

## Result

The NVMe storage, persistent mounting, Samba configuration, and network file sharing are all operational.