# Single GPU Passthrough
This guide aims to help you to create a VM with GPU passthrough on Linux<br>

This guide was written for ArchLinux w/ Linux-zen kernel and a RTX 2080 but it should work for any other configurations awa others linux arch or debian based distros (tho not recommended). Feel free to try it out anyways !<br>

# Global requirements
For all of those VMs, you'll need:
all these conditions must be met before getting into the first section (tldr you cannot follow this tutorial if your pc does not have UEFI)
- Intel VT-d or AMD-Vi or whatever its called enabled in the BIOS
- IOMMU enabled as well
- [kernel parametres with IOMMU enabled](Global/IOMMU.md) <- should follow this before going to the next section
- libvirt + qemu installed
- virt-manager (optional but highly recommended cuz fuck command line) 

# Getting started:
[Windows 10/11](Windows%2010.md)<br>
[Windows 7](Windows%207.md) <- unstable asf please use windows 10 i swear<br>  
Optional:<br>
[Hide the VM (win10)](Hide.md) <- required if you wanna play valorant or anticheat-dependant games


# Obligatory help related section
You can ask for help here if you're in need: <br>
[Issues page (preferred)](https://github.com/nixuge/Single_GPU_Passthrough/Issues)<br>
[r/vfio subreddit](https://reddit.com/r/vfio)<br>
[Personal contact](https://nixuge.me)<br>

[click me](CREDITS.md)<br>
If for some obscure reasons the guide does not work for you I recommend checking out some of those. Many of the resources here are just taken up from these and reorganized in a -readeable- way (i basically did all the research work for you now star the repo)