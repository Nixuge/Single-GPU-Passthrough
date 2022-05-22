# best part of this tutorial so far, have fun fucking up your vm 
## You have 2 choices to get some input methods on your VM:


# Adding individual devices to the VM (facility solution)
**advantages:**
- easy to find and add
- more compatible 
  
**inconvenients:**
- No plug & play
- Have to edit the whole config if you change one of your USB devices (ex: switching pc case)
  
## How to do it:
<img src="images/AddUsbDevices.png" width="400"/><br>


# Passing through an entire controller (made me racist)
**advantages:**
- Passing the controller itself, it works just like it would normally (plug & play etc)

**inconvenients:**
- Usually doesn't work well on Windows 7 (due to USB 3 drivers)
- Front ports on some motherboards does not use the same usb controller
- May be a bit harder to find in the list if you're not sure what to look for

## How to do it:
<img src="images/AddController.png" width="400"/><br>
If you have an AMD CPU, it should be easy enough to find your usb controller (usually its called by the same name as your cpu soc: zeppelin for 2xxx, matisse for 3xxx, etc..)<br>