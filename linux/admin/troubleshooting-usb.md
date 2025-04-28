# Adding a USB Dongle in Linux

To add and troubleshoot a USB dongle in Linux, follow these steps:

## 1. Check USB Device Detection
Run the following command to verify if the USB dongle is detected:
```bash
lsusb
```
This will list all connected USB devices. Look for your dongle in the output.

## 2. Check Kernel Messages
Inspect kernel logs for USB-related messages:
```bash
sudi dmesg | grep -i usb
sudo dmesg | grep -i bluetooth
```
This can help identify if the device was recognized or if there are any errors.

## 3. Verify Device Node
Check if the device node was created:
```bash
ls /dev | grep usb
```

## 4. Install Required Drivers
If the dongle requires specific drivers, install them using your package manager. For example:
```bash
sudo apt install <driver-package-name>  # For Debian/Ubuntu
sudo yum install <driver-package-name>  # For CentOS/RHEL
```

## 5. Load Kernel Modules
If necessary, load the appropriate kernel module:
```bash
sudo modprobe <module-name>
#
sudo modprobe btusb
sudo systemctl restart bluetooth.service
```

## 6. Test the USB Dongle
Test the functionality of the dongle based on its purpose (e.g., network, storage, etc.).

## 7. Troubleshooting
- Use `lsusb -v` for detailed information about the device.
- Check permissions for the device node.
- Refer to the manufacturer's documentation for additional setup steps.

### 7.1 My Bluetooth Device 

TP-Link Nano USB Adapter 
Bluetooth 5.3 - UB500


## References
- `man lsusb`
- Linux kernel documentation
- Manufacturer's support page