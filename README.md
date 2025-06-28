# Radxa Zero3 DMA32 unchached heap
Zero3 DMA32 heap deb packages
Use these packages to create an uncahched 32-bit DMA heap. 
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
sudo apt-mark hold linux-image-6.1.0-rk356x
sudo apt-mark hold linux-headers-6.1.0-rk356x
```
Test:
```bash
ls /dev/dma_heap
```
returns: `reserved  system  system-dma32  system-uncached-dma32`
