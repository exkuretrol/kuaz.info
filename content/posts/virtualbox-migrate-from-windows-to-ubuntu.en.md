---
title: "Migrate from Windows VirtualBox to Ubuntu headless VirtualBox"
date: 2021-08-18T22:13:55+08:00
draft: false
---

First, we import .ova file, which exported from Windows's VirtualBox.
```bash
# import Virtual Machine from VirtualBox exported file.
vboxmanage import /mnt/ntfs/MCU-ASIS.ova
```

Then, modify VMs network bridge config to the following below:
```bash
# make sure you use command ip addr to check your network bridge name
vboxmanage modifyvm MCU-ASIS \
  --nic1 bridged
  --nictype1 82540EM
  --bridgeadapter1 ens2p0
```

Finally, we use the following commands to control the virtual machine.
```bash
# Start headless Ubuntu virtual machine
vboxmanage startvm MCU-ASIS --type headless
# Stop Virtual Machine
vboxmanage controlvm MCU-ASIS poweroff
```

How to get ip of the virtual machine?
```bash
# Get Virtual Machine ip
arp -a
# Alternative
ip neigh
```
