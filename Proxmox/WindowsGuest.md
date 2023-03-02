# Windows Guest Best Practice
## Prepare
To obtain a good level of performance, we will install the Windows VirtIO Drivers during the Windows installation.

- Create a new VM, select "Microsoft Windows 10/2016/2019" as Guest OS and enable the "Qemu Agent" in the System tab.
- Mount your Windows 10 ISO in the CDROM drive
- For your virtual hard disk select "SCSI" as bus with "VirtIO SCSI" as controller.
- Set "Write back" as cache option for best performance (the "No cache" default is safer, but slower) and tick "Discard" to optimally use disk space (TRIM).
- Configure your memory settings as needed, continue and set "VirtIO (paravirtualized)" as network device, finish your VM creation.
- For the VirtIO drivers, upload the driver ISO to your storage, create a new CDROM drive with Bus "IDE" and number 0. Load the Virtio Drivers ISO in the new virtual CDROM drive.
- Now your ready to start the VM, just follow the Windows installer.

## Launch Windows install
- After starting your VM launch the noVNC console
- Follow the installer steps until you reach the installation type selection where you need to select "Custom (advanced)"
- Now click "Load driver" to install the VirtIO drivers for hard disk and the network.
  - Hard disk: Browse to the CD drive where you mounted the VirtIO driver and select folder "vioscsi\w10\amd64" and confirm. Select the "Red Hat VirtIO SCSI pass-through controller" and click next to install it. Now you should see your drive.
  - Network: Repeat the steps from above (click again "Load driver", etc.) and select the folder "NetKVM\w10\amd64", confirm it and select "Redhat VirtIO Ethernet Adapter" and click next.
  - Memory Ballooning: Again, repeat the steps but this time select the "Balloon\w10\amd64" folder, then the "VirtIO Balloon Driver" and install it by clicking next.
- Choose the drive and continue the Windows installer steps.

## Post Install
### Guest Agent
If you enabled the Qemu Agent option for the VM the mouse pointer will probably be off after the first boot.

To remedy this install the "Qemu Guest Agent". The installer is located on the driver CD under guest-agent\qemu-ga-x86_64.msi.

### Drivers and Services
The easiest way to install missing drivers and services is to use the provided MSI installer. It is available on the driver CD since version "virtio-win-0.1.173-2".

Run the "virtio-win-gt-x64.msi" file located directly on the CD. If you do not plan to use SPICE you can deselect the "Qxl" and "Spice" features. Restart the VM after the installer is done.

After all this the RAM usage and IP configuration should be shown correctly in the summary page of the VM.
