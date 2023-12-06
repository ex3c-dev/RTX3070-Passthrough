# RTX3070-Passthrough
A basic guide to get a gpu passthrough to a windows 11 vm in proxmox.
## Basic GPU Passthrough
Follow the guide in the official proxmox [wiki](https://pve.proxmox.com/wiki/PCI_Passthrough).
## Modifying your vBios
### GPU-Z
Dump your vBios rom file using GPU-Z or find one using the official [database](https://www.techpowerup.com/vgabios/)
### Linux (Alternative)
Dump it manually:
```
cd /sys/bus/pci/devices/0000:01:00.0/
echo 1 > rom
cat rom > /tmp/image.rom
echo 0 > rom
```
### Editing the rom file.
Using a hex editor of your choice (Recommandation: Bless) open the rom and search for `VIDEO`. Look for a string that resembles: `U...K7400.L.w.VIDEO ...p._...IBM VGA Compatible`. Remove everything before the U and save your rom in a new file.
### Pass the rom file to the vm.
1. Upload the rom file to your proxmox server to the following folder: `/usr/share/kvm`
2. Go to your vm config in `/etc/pve/qemu-server` and add your romfile to the pci passthrough `romfile=<RomFile>`.
3. Your vm should now boot without a black screen.

### Troubleshooting
#### Audio Problems
If you are having audio problems and the HDMI Device is not detected as an audio device try reinstalling the Nvidia driver. This is common when upgrading your gpu.
