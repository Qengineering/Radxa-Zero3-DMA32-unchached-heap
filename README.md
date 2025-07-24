# Radxa Zero3 4 GB RAM limit
Zero3 4 GB RAM deb packages
The Rockchip RGA library only works with Radxa Zero 3 devices with 4 GB or less RAM. 
Above 4 GB, you are facing:
- DMA memory mapping issues:
    - The RGA hardware can't access the full 8 GB memory map because of address range limitations.
    - The 32-bit IOVA (Input/Output Virtual Address) used by RGA may wrap or overflow.
- DMA Heap fallback issues:
    - The system typically uses /dev/dma_heap/system or /dev/dma_heap/system-uncached.
    - On some kernels, the 8 GB configuration causes failures or kernel panics on RGA operations.

----------------

## Installation.
The packages are compiled for the [Ubuntu 24.04](https://github.com/Joshua-Riek/ubuntu-rockchip) OS.
```bash
sudo dpkg -i linux-buildinfo-6.1.0-1027-rockchip_6.1.0-1027.27_arm64.deb
sudo dpkg -i linux-rockchip-headers-6.1.0-1027_6.1.0-1027.27_arm64.deb
sudo dpkg -i linux-headers-6.1.0-1027-rockchip_6.1.0-1027.27_arm64.deb
sudo dpkg -i linux-image-6.1.0-1027-rockchip_6.1.0-1027.27_arm64.deb
sudo dpkg -i linux-modules-6.1.0-1027-rockchip_6.1.0-1027.27_arm64.deb  
sudo dpkg --configure -a
sudo sync
sudo reboot
```
To hold the kernel packages so they aren't automatically upgraded.
```bash
dpkg --list | grep linux-image
sudo apt-mark hold linux-image-6.1.0-1027-rockchip
```
Better to enlarge the CMA (Contiguous Memory Allocator).
```bash
sudo nano /boot/extlinux/extlinux.config
# in append:
cma=256M@0x20000000 
```
Test:
```bash
ls /dev/dma_heap
```
returns: `reserved  system  system-dma32  system-uncached-dma32`
