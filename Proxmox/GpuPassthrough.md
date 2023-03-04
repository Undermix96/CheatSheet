# Configuring Proxmox
This guide assumes you already have at the very least, installed Proxmox on your server and are able to login to the WebGUI and have access to the server node's Shell terminal. If you need help with installing base Proxmox, I highly recommend the official "Getting Started" guide and their official YouTube guides.

## Step 1: Configuring the Grub

Assuming you are using an Intel CPU, either SSH directly into your Proxmox server, or utilizing the noVNC Shell terminal under "Node", open up the /etc/default/grub file. I prefer to use nano, but you can use whatever text editor you prefer.

```
nano /etc/default/grub
```

Look for this line:  
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
```

Then change it to look like this:

For Intel CPUs:  
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

For AMD CPUs:  
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
```

**IMPORTANT ADDITIONAL COMMANDS**

You might need to add additional commands to this line, if the passthrough ends up failing. For example, if you're using a similar CPU as I am (Xeon E3-12xx series), which has horrible IOMMU grouping capabilities, and/or you are trying to passthrough a single GPU.

These additional commands essentially tell Proxmox not to utilize the GPUs present for itself, as well as helping to split each PCI device into its own IOMMU group. This is important because, if you try to use a GPU in say, IOMMU group 1, and group 1 also has your CPU grouped together for example, then your GPU passthrough will fail.

Here are my grub command line settings:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction nofb nomodeset video=vesafb:off,efifb:off"
```

For more information on what these commands do and how they help:  
A. Disabling the Framebuffer: video=vesafb:off,efifb:off  
B. ACS Override for IOMMU groups: pcie_acs_override=downstream,multifunction

When you finished editing /etc/default/grub run this command:  
```
update-grub
```

## Step 2: VFIO Modules

You'll need to add a few VFIO modules to your Proxmox system. Again, using nano (or whatever), edit the file /etc/modules

```
nano /etc/modules
```
Add the following (copy/paste) to the /etc/modules file:

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

Then save and exit.

## Step 3: IOMMU interrupt remapping

I'm not going to get too much into this; all you really need to do is run the following commands in your Shell:
```
echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
```

```
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

## Step 4: Blacklisting Drivers

We don't want the Proxmox host system utilizing our GPU(s), so we need to blacklist the drivers. Run these commands in your Shell:

```
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
```

```
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
```

```
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
```
