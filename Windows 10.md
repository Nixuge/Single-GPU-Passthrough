# Requirements
- w10/11 iso
- virt-manager (recommended, again)
- time

# Basic VM Setup:
First, create a VM:<br>
<img src="assets/Global/CreateVmButton.png" width="400"/><br>

<img src="assets/Global/CreateVm1.png" width="400"/>
<img src="assets/win10/CreateVm2.png" width="400"/>

Enter whatever value you want here (half my amount of ram in this case / can do more or less), you can change that later anyway<br>
<img src="assets/Global/CreateVm3.png" width="400"/><br>

Use any disk size you want (superior to 60 just in case)<br>
<img src="assets/Global/CreateVm4.png" width="400"/><br>

On the last step, make sure to hit "Customize configuration before install"<br>
<img src="assets/win10/CreateVm5.png" width="400"/><br>

After that you should be redirected on this screen.<br>
On firmware, select UEFI x86_64 like in the screenshot **not bios**<br>
(secboot is needed for Windows 11, technically not for 10 )<br>
<img src="assets/win10/CreateVmConfigure1.png" width="800"/><br>

If you're using Windows 11, you **need** to setup the TPM thing.<br>
<img src="assets/win10/CreateVmConfigure2.png" width="800"/><br>


You can tweak a few parameters if you want, like more CPU cores, but for now we don't need anything more.<br>

Click on **"Begin Installation"**, and setup windows like you would on a normal computer.<br>
Once you get on the desktop, shutdown the VM.


# Hooks:
Those will basically hook your graphic card into the VM and and vice versa when you shut it down.<br>
[Click this before continuing](Global/hooks.md)


# GPU Setup on the VM:
Once the hooks are done, we can finally add our GPU to the VM itself.<br>

## Remove the included video devices
*red = remove*<br> 
*yellow = optional*<br>
<img src="assets/win10/VmGpuConfigure1.png" width="200"/><br>

## Add the GPU
Add all of your GPU entries from the IDs you got inside the "hooks" section (1 by 1).<br>
<img src="assets/win10/VmGpuConfigure2.png" width="800"/><br>

## DONE!!!
At this point your VM is now working (in most cases it should, if not gl lmao)<br>
You can see the below section if you want your USB ports to work on the VM

# USB devices:
[Make em work](Global/USB.md)
