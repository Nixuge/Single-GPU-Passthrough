# Requirements
- A working Windows 10/11 ISO
- libvirt + qemu installed
- Virtual Machine Manager (Optional, recommended)

# Basic VM Setup:
First, create a VM:<br>
<img src="images/Global/CreateVmButton.png" width="200"/><br>

<img src="images/Global/CreateVm1.png" width="200"/>
<img src="images/win10/CreateVm2.png" width="200"/>
<img src="images/win10/CreateVm3.png" width="200"/><br>

You can change the disk size to whathever you want here<br>
<img src="images/Global/CreateVm4.png" width="200"/><br>

On the last step, make sure to hit "Customize configuration before install"<br>
<img src="images/win10/CreateVm5.png" width="200"/><br>

After that you should have this screen. On firmware, select UEFI x86_64 like in the screenshot<br>(secboot is needed for Windows 11, technically not for 10)<br>
<img src="images/win10/CreateVmConfigure1.png" width="400"/><br>

If you're using Windows 11, you **need** to setup a TPM.<br>
<img src="images/win10/CreateVmConfigure2.png" width="400"/><br>


You can tweak a few parameters if you want, like more CPU cores, but for now we don't need anything more.<br>
Click on "Begin Installation", and setup windows like you'd on a normal computer.<br>
Once you're done installing, shutdown the VM.


# Hooks:
[See this page on how to setup hooks](Global/hooks.md)


# GPU Setup on the VM:
Once the hooks are done, we can finally add the GPU to the VM itself.<br>

## Remove the included video devices
*red = remove, yellow = optional*<br>
<img src="images/win10/VmGpuConfigure1.png" width="100"/><br>

## Add the GPU
Add all of your GPU entries from the IDs you got inside the "hooks" section (1 by 1).<br>
<img src="images/win10/VmGpuConfigure2.png" width="400"/><br>


# USB devices:
If you booted up your VM at this point, it should be working but you'll notice you don't have any input method. [You can fix that right here](Global/USB.md)
