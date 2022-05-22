# Initial setup:
We're going to use hooks to run instructions before and after running our VMs.<br>
The setup is pretty straight forward, [the instructions are taken from here](https://passthroughpo.st/simple-per-vm-libvirt-hooks-with-the-vfio-tools-hook-helper/). 
Just the following commands:<br>
```sh
sudo mkdir -p /etc/libvirt/hooks
sudo wget 'https://raw.githubusercontent.com/PassthroughPOST/VFIO-Tools/master/libvirt_hooks/qemu' \
    -O /etc/libvirt/hooks/qemu
sudo chmod +x /etc/libvirt/hooks/qemu
sudo service libvirtd restart
```

Next, we have to setup the actual hooks for our vm.<br>
We aim for a file structure like this:<br>

```
/etc/libvirt/hooks
├── qemu
└── qemu.d
    └── {VM Name}
        ├── prepare
        │   └── begin
        │       └── start.sh
        └── release
            └── end
                └── revert.sh
```

To achieve this, run these commands (replace {VM Name} by the name of your vm):
```sh
sudo mkdir -p /etc/libvirt/hooks/qemu.d/{VM Name}/prepare/begin
sudo mkdir -p /etc/libvirt/hooks/qemu.d/{VM Name}/release/end
```

# Ensure that the IOMMU groups are valid
[More info](https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF#Ensuring_that_the_groups_are_valid)<br><br>
Run:
```sh
#!/bin/bash
shopt -s nullglob
for g in $(find /sys/kernel/iommu_groups/* -maxdepth 0 -type d | sort -V); do
    echo "IOMMU Group ${g##*/}:"
    for d in $g/devices/*; do
        echo -e "\t$(lspci -nns ${d##*/})"
    done;
done;
```
You should see some long output with the actual IOMMU groups. <br>
You need to find your GPU IDs in this mess and ensure that **all your gpu ids** are either inside **the same group** or separated into **multiple groups**.<br>

In case of multiple groups, **make sure** that the ID is isolated and alone in the group, if it's not, [issues section](https://github.com/nixuge/Single_GPU_Passthrough/Issues)<br>

As an example, ive got this output:<br>
```
IOMMU Group 24:
	24:00.0 Network controller [0280]: Intel Corporation Wireless 8260 [8086:24f3] (rev 3a)
IOMMU Group 25:
	26:00.0 USB controller [0c03]: ASMedia Technology Inc. ASM1142 USB 3.1 Host Controller [1b21:1242]
IOMMU Group 26:
	27:00.0 VGA compatible controller [0300]: NVIDIA Corporation TU104 [GeForce RTX 2080] [10de:1e82] (rev a1)
IOMMU Group 27:
	27:00.1 Audio device [0403]: NVIDIA Corporation TU104 HD Audio Controller [10de:10f8] (rev a1)
IOMMU Group 28:
	27:00.2 USB controller [0c03]: NVIDIA Corporation TU104 USB 3.1 Host Controller [10de:1ad8] (rev a1)
IOMMU Group 29:
	27:00.3 Serial bus controller [0c80]: NVIDIA Corporation TU104 USB Type-C UCSI Controller [10de:1ad9] (rev a1)
```

Note that my graphic card is splitted in 4 different groups because i'm using the **zen kernel**. If you're using the regular one, all of them should be inside of one. Both of those options are **fine**.<br><br>
In this example, the IDs we're looking for are:
- 27:00.0
- 27:00.1
- 27:00.2
- 27:00.3



## Startup script
This script is going to be used when starting the VM. Its utilized to unbind the GPU from your host machine. [Mostly copy pasted from joeknock90's guide](https://github.com/joeknock90/Single-GPU-Passthrough#start-script)<br>

Put the following script in `/etc/libvirt/hooks/qemu.d/{VM Name}/prepare/begin/start.sh` and configure it to your needs<br>
```sh
#!/bin/bash
# Helpful to read output when debugging
set -x

# Stop display manager
#gdm = gnome, sddm = kde, lightdm = most others, etc....
systemctl stop gdm.service
## Comment the following line if you don't use GDM
killall gdm-x-session

# Unbind VTconsoles
echo 0 > /sys/class/vtconsole/vtcon0/bind
echo 0 > /sys/class/vtconsole/vtcon1/bind

# Unbind EFI-Framebuffer
echo efi-framebuffer.0 > /sys/bus/platform/drivers/efi-fr>

# Avoid a Race condition by waiting 2 seconds. This can b>
sleep 2

#unload the nvidia drivers
# Change those to the AMD drivers if you've got an AMD card
modprobe -r nvidia_drm
modprobe -r nvidia_modeset
modprobe -r drm_kms_helper
modprobe -r drm
modprobe -r nvidia_uvm
modprobe -r nvidia


# Unbind the GPU from display driver
#CHANGE THOSE TO THE IDS YOU SAVED ON THE LAST STEP (replace : and . with _)
virsh nodedev-detach pci_0000_27_00_0
virsh nodedev-detach pci_0000_27_00_1
virsh nodedev-detach pci_0000_27_00_2
virsh nodedev-detach pci_0000_27_00_3

# Load VFIO Kernel Module
modprobe vfio-pci
```


## Shutdown script
This script is going to be used when stopping the VM. [still joeknock90's guide](https://github.com/joeknock90/Single-GPU-Passthrough#start-script)<br>

Put the following script in `/etc/libvirt/hooks/qemu.d/{VM Name}/release/end/revert.sh` and configure it to your needs
```sh
#!/bin/bash
set -x

# Re-Bind GPU to Nvidia Driver
#Change to the IDS in the last step, but in inverted order
virsh nodedev-reattach pci_0000_27_00_3
virsh nodedev-reattach pci_0000_27_00_2
virsh nodedev-reattach pci_0000_27_00_1
virsh nodedev-reattach pci_0000_27_00_0

# Reload nvidia modules
#change to AMD if AMD
modprobe nvidia
modprobe nvidia_modeset
modprobe nvidia_uvm
modprobe nvidia_drm

# Rebind VT consoles
echo 1 > /sys/class/vtconsole/vtcon0/bind
# Some machines might have more than 1 virtual console. A>
#echo 1 > /sys/class/vtconsole/vtcon1/bind

nvidia-xconfig --query-gpu-info > /dev/null 2>&1
echo "efi-framebuffer.0" > /sys/bus/platform/drivers/efi->

# Restart Display Manager
#same as before, change if different display manager
systemctl start gdm.service
```

## Permissions /!\ **IMPORTANT**
Run:
Replace VM Name by your vm's name<br>
```sh
sudo chmod +x /etc/libvirt/hooks/qemu.d/{VM Name}/prepare/begin/start.sh
sudo chmod +x /etc/libvirt/hooks/qemu.d/{VM Name}/release/end/revert.sh
```

# Alternatives

If the previous didn't work for you (it should, you can open an issue) <br>
If the issue pages didnt solve your problem, you can try out the [rising prism hooks](https://gitlab.com/risingprismtv/single-gpu-passthrough), they're specially made for AMD GPU users.

# Go back
[Go back to the w10/11 guide](Windows%2010.md)<br>
[same but for windows 7](Windows%207.md)<br> 