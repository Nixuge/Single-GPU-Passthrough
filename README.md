# Single GPU Passthrough
Welcome to this guide to help you make a VM w GPU Passthrough on Linux.<br>
This guide was written on ArchLinux with the Linux-Zen kernel and an RTX 2080.<br>
Altho not recommended, most thing here should on pretty much any other recent Linux distro. Feel free to try it out anyways !

# Credits
[Click here to get to the credits.](CREDITS.md)<br>
I recommend checking some of those out if this guide doesn't work for some reason. A lot of the ressources are taken straight up from those and adapted a bit.

# Help
If you need help, you can ask for support here:<br>
[Issues page (prefeered)](https://github.com/nixuge/Single_GPU_Passthrough/Issues)<br>
[The r/vfio subreddit](https://reddit.com/r/vfio)<br>
[Personal contact](https://nixuge.me)<br>

# Global requirements
For all of those VMs, you'll need:
- Intel VT-d or AMD-Vi enabled in BIOS
- IOMMU enabled in BIOS
- [kernel parametres with IOMMU enabled](Global/IOMMU.md)
- libvirt + qemu installed
- Virtual Machine Manager (Optional, recommended)

# Setups
[Windows 10/11](Windows%2010.md)<br>
[Windows 7](Windows%207.md)<br>
Additional:<br>
[Hide the VM (win10)](Hide/README.md)
