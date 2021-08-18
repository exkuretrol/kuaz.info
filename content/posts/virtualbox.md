---
title: "Virtualbox"
date: 2021-08-18T22:13:55+08:00
draft: true
---


```bash
# import Virtual Machine from VirtualBox exported file.
vboxmanage import /mnt/ntfs/MCU-ASIS.ova
```

```bash
# modify Windows Network bridge setting to linux
vboxmanage modifyvm MCU-ASIS \
  --nic1 bridged
  --nictype1 82540EM
  --bridgeadapter1 ens2p0
```

```bash
# Start headless Ubuntu virtual machine
vboxmanage startvm MCU-ASIS --type headless
# Stop Virtual Machine
vboxmanage controlvm MCU-ASIS poweroff
```

```bash
# Get Virtual Machine ip
arp -a
# Alternative
ip neigh
```
