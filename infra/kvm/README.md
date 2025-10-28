## VM installation on a stand-alone KVM server

This documentation presumes that you have an existing physical server and you wish to setup a Virtual Machine on it.

This also assumes that you have the underlying hypervisor already configured and you are using LVM for VM storage.

The main purpose of this document is to allow us to re-build the VM from scratch should it be required.

# Fetch AlmaLinux 10 ISO

We presume `/iso` is adequate for storage, even if temporary.

```
mkdir /iso
wget https://mirror.aarnet.edu.au/pub/almalinux/9.6/isos/x86_64/AlmaLinux-9.6-x86_64-minimal.iso -O /iso/AlmaLinux-9.6-x86_64-minimal.iso
```

# Allocate storage

In this example, the host has 2 PV's and 2 VGs (SSD vs HDD storage split up).

We want the OS to be installed on the quick storage (`vg0` PV), and bulk storage on the HDD storage (`spinningrust` PV).

Create a LV for the OS:

```
lvcreate -L 50G -n vm-pompyboard-os vg0
```

Create a LV for the bulk storage:

```
lvcreate -L 200G -n vm-pompyboard-large spinningrust
```

# Create the VM

The sample below is purely unique to our env, `br218` is the name of our bridge.

```
virt-install \
  --name vm-pompyboard \
  --vcpus 4 \
  --memory 4096 \
  --cpu host \
  --disk path=/dev/vg0/vm-pompyboard-os,format=raw,bus=virtio,discard=unmap,cache=none,io=native \
  --disk path=/dev/spinningrust/vm-pompyboard-large,format=raw,bus=virtio,cache=none,io=native \
  --cdrom /iso/AlmaLinux-9.6-x86_64-minimal.iso \
  --os-variant almalinux9 \
  --network bridge=br218,model=virtio \
  --boot cdrom,hd \
  --noautoconsole
```

You should see something like this:

```
Starting install...
Creating domain... |    0 B  00:00:00

Domain is still running. Installation may be in progress.
You can reconnect to the console to complete the installation process.
```

Connect to the console next:

```
virsh console vm-pompyboard
```

OR connect via `virt-manager` to complete the OS installation.

Please ensure you set the server to use UTC time!

Ensure autostart is configured post install:

```
virsh autostart vm-pompyboard
```

# Partitions

Keep it simple, use LVM (presuming 50GiB to play with).

ext4 filesystem is preferred.

```
/boot 1GiB (ext4)
swap 2GiB
/ 47GiB (ext4)
```

We will setup the HDD/larger storage later.

# Post installation tasks

Ideally via `virsh console`, login to the VM and copy your public SSH key to the VM.

Once done, fix perms:

```
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

Randomise the root password (you can always break in to the VM via single user mode if required).

Disable selinux:

```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

Run updates and reboot:

```
dnf update -y
reboot
```

Create a new PV and LV for the HDD/larger device:

```
pvcreate /dev/vdb
vgcreate data /dev/vdb
```

Create a storage LV and format it with xfs:

```
lvcreate -n storage -l 100%FREE data
mkfs.xfs /dev/data/storage
```

Mount the LV:

```
mkdir /storage
echo "UUID=$(blkid|grep -i storage | cut -d\" -f2)   /storage   xfs   defaults,noatime   0 0" >> /etc/fstab
systemctl daemon-reload
mount -a
```
