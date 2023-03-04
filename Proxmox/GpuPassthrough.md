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

## Step 5: Adding GPU to VFIO

Run this command:

```
lspci -v
```

Your shell window should output a bunch of stuff. Look for the line(s) that show your video card. It'll look something like this:
```
01:00.0 VGA compatible controller: NVIDIA Corporation GP104 [GeForce GTX 1070] (rev a1) (prog-if 00 [VGA controller])

01:00.1 Audio device: NVIDIA Corporation GP104 High Definition Audio Controller (rev a1)
```

Make note of the first set of numbers (e.g. 01:00.0 and 01:00.1). We'll need them for the next step.

Run the command below. Replace 01:00 with whatever number was next to your GPU when you ran the previous command:
```
lspci -n -s 01:00
```
Doing this should output your GPU card's Vendor IDs, usually one ID for the GPU and one ID for the Audio bus. It'll look a little something like this:
```
01:00.0 0000: 10de:1b81 (rev a1)

01:00.1 0000: 10de:10f0 (rev a1)
```
What we want to keep, are these vendor id codes: 10de:1b81 and 10de:10f0.

Now we add the GPU's vendor id's to the VFIO (remember to replace the id's with your own!):

```
echo "options vfio-pci ids=10de:1b81,10de:10f0 disable_vga=1"> /etc/modprobe.d/vfio.conf
```

Finally, we run this command:

```
update-initramfs -u
```

And restart:
```
reset
```

Now your Proxmox host should be ready to passthrough GPUs!
