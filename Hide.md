# Hide the VM 

### By default, the VM isn't hidden at all and can be easily detected by some popular anti cheats (and can eventually ban your host machine cough cough vanguard). We want to avoid this.

## Disclaimer
Some games **will** still detect the VM (except maybe if you do some obscure tricks, and even then you're not guaranteed to not get detected/silent flagged). I'm mostly talking about Valorant and their fucking kernel-side AC which litteraly still doesnt work on plain windows after 4 years of developpement. Tbh just avoid valorant or battlefield V.


# Avoid code 43 on nvidia cards / Allow screen rotation on AMD GPUs

### This isn't needed anymore if you got updated drivers as nvidia removed those restrictions, but still leaving that here.
### AMD still cant make their driver work even when installed properly so heres the fix for 144hz+ and screen rotation

Edit your VM's xml (google "virtmanager edit xml" if you dont know how to do)
```xml
    <hyperv mode="custom">
        ...
        ...
        <relaxed state='on'/>
        <vapic state='on'/>
        <spinlocks state='on' retries='8191'/>
        <vendor_id state='on' value='whateveryouwant'/>
    </hyperv>
    <kvm>
        <hidden state='on'/>
    </kvm>
    <vmport state='off'/>
    <ioapic driver='kvm'/>

```


# Disable the hypervisor

### Note: this **WILL** make your VM slower, sometimes by a lot. For some games it doesn't matter that much, but it's still recommended that you leave that on if you can.

Find your cpu tag, and inside of it add the 2 features lines in the example below:
```xml
<cpu mode="..." check="..." migratable="...">
    ...
    <feature policy="disable" name="hypervisor"/>
    <feature policy="require" name="invtsc"/>
</cpu>
```

# Other [pafish](https://github.com/a0rtega/pafish) fixes

## Note: Not sure if those are used in real detection, but better safe than sorry.

### [\*] Checking mouse click activity ... traced!<br>[\*] Checking mouse double click activity ... traced!<br>[\*] Checking operating system uptime using GetTickCount() ... traced!
> Those checks are pretty inconsistent and i've seen the mouse ones getting triggered even with the logitech drivers on and installed. You can just ignore them.


### [\*] Checking the difference between CPU timestamp counters (rdtsc) forcing VM exit ... traced!
> Haven't tested any myself for this one, so here are a few possible options: <br>
> https://github.com/SamuelTulach/BetterTiming <br>
> https://github.com/WCharacter/RDTSC-KVM-Handler


### [\*]Checking hypervisor bit in cpuid feature bits ... traced!<br>[\*]Checking cpuid hypervisor vendor for known VM vendors ... traced!
> See [#Disable the hypervisor](#disable-the-hypervisor)


### [\*] Scsi port->bus->target id->logical unit id-> 0 identifier ... traced!<br>[\*] Reg key (HKLM\HARDWARE\Description\System "SystemBiosVersion") ... traced!

<details> 
<summary> ### Temporary fix (every reboot, not recommended): </summary>

> Change the following registery keys:<br>
> ### `Computer\HKEY_LOCAL_MACHINE\HARDWARE\DEVICEMAP\Scsi\Scsi Port 0\Scsi Bus 0\Target Id 0\Logical Unit Id 0`
> > Identifier: "CT1000MX500SSD1"<br>
>
> ### `Computer\HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System`
> > SystemBiosVersion: set as something else 
>
> ### `Computer\HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS`
> > SystemProductName: "To Be Filled By O.E.M."<br>
> > SystemVersion: "To Be Filled By O.E.M."<br>
> > BIOSVendor: "American Megatrends Inc."<br>
> > BIOSVersion: "P4.40"<br>
> 
</details>

#### Permanent proper fix:
> See [#Recompile Qemu with fixes](#recompile-qemu-with-fixes)

#### Permanent easier fix:
> [Download this file](assets/hide/HideVM.reg) and run it on startup (win+r then shell:startup)

### [\*] cpuid CPU brand string 'QEMU Virtual CPU' ... traced!
> Change your CPU model to something else, usually "Host Passthrough" is recommended.



# Recompile Qemu with fixes
[Click here to see the fixes to make to hide eveything](https://github.com/nbaertsch/qemu-git-patched-pkgbuild/blob/6c5092097060b83542965b1362c7afbc17e0dade/PKGBUILD#L43)<br>
Note that this PKGBUILD is 2 years old now, I don't really need to hide my VMs that much so i'm just not using it.<br>
If you REALLY want to hide everything, I recommend making your own PKGBUILD from the official qemu-git one. Just copy pasting the fix lines from the old PKGBUILD into the new one should be enough, but can't say for sure