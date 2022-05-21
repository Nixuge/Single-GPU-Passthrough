# Enable IOMMU on kernel parameters

Edit /etc/default/grub and add

`GRUB_CMDLINE_LINUX_DEFAULT="... intel_iommu=on iommu=pt ..."`<br>

Or<br>

`GRUB_CMDLINE_LINUX_DEFAULT="... amd_iommu=on iommu=pt ..."`<br>

Depending on your CPU manufacturer.<br>

Once that's done, regen your grub config with
```sh
grub-mkconfig -o /boot/grub/grub.cfg
```
And reboot