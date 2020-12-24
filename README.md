# Gondola

## Description

This project should provide an ansible playbook that provisions a
emulated raspberry pi virtual machine on the local KVM machine.

A quick over view, this project gets the following files in the table bellow, converts the `.img` file to `.qcow2` (allows us to snapshot guest)
and puts the files in your libvirt images folder. Then creates the kvm guest and an initial snapshot. From there you can clone that initial image for development related tasks.

| File                                     | Purpose                                                                            |
| ---------------------------------------- | ---------------------------------------------------------------------------------- |
| 2020-05-27-raspios-buster-lite-armhf.img | The [image](https://www.raspberrypi.org/downloads/) containng the operating system |
| kernel-qemu-4.19.50-buster               | The [kernal](<https://en.wikipedia.org/wiki/Kernel_(operating_system)>)            |
| versatile-pb-buster.dtb                  | The [device tree blob](https://superuser.com/a/736988)                             |

You can instead use stretch by uncommenting that section in `./vars.yml`, buster has issues installing docker ATM.

## Installation

Make sure you have `libvirt-dev` and `qemu-system-arm` installed as well.

`pip install -r requirements.txt`

## Usage

`sh main.sh`

At this point you should be able to ping your pi

`ping raspberrypi.local`

You might drop into the guest and enable openssh-server username is `pi` password is `raspberry`

Expand the disk
`sudo raspi-config --expand-rootfs`

Enable openssh-server
`sudo raspi-config`

Then enable the openssh-server

And then from your host machine

`ssh-copy-id -i ~/.ssh/id_ed25519.pub pi@raspberrypi.local`

Now you should be able ssh in.

`ssh pi@raspberrypi.local`

Best to take a snapshot while we are here.

`virsh snapshot-create-as --domain pi __name "host_ssh_key_added_to_pi_user"`

Do any more processing to the base image that you would like, idealy before you deploy these images in the wild you add
maybe `ufw` and `fail2ban`

Now lets create your actual image you'll use for which ever project your working on.

Power off your machine if it isn't already.

Now lets clone, change `my_dev_pi` to what ever you need it to be.

`virt-clone --original pi --auto-clone -n my_dev_pi`

### Once your ready to run your image on physical hardware.
Shut  down the vm if it is started.

`virsh destroy pi`

Convert .qcow2 back to .img

`qemu-img convert /var/lib/libvirt/images/2020-05-27-raspios-buster-lite-armhf.qcow2 -O raw ~/Downloads/disk.img`

## Roadmap

- [x] Ansible Setup with Connection to Specified Host - Hello World
  - [x] Make sure KVM NAT network is accessible with DNS
- [x] Download Raspberry Pi Image
- [x] Download the necessary kernals
- [x] Asnible Jinja Templete Libvirt Guest XML
- [x] Initial provision of SSH key and any required packages
  - [ ] Clone the VM this guest can be developed against.
  - [ ] Dump Image to Be burned to USB
- [ ] Develop Image (our Firmware)
- [ ] Create a Firmware Release (qcow2 back to bootable image)

## Resizing Disk Ver2

The stretch image for some reason wont expand using the
`sudo raspi-config --expand-rootfs` the instructions below are from [this](https://raspberry-projects.com/pi/pi-operating-systems/raspbian/troubleshooting/expand-filesystem-issues)
article.

Instead open the disk in fsdisk

`sudo fdisk /dev/vda`

Then hit the following

```
Press 'd' > Enter

Press '2' > Enter

Now re-create it:

Press 'n' > Enter

Press 'p' > Enter

Press '2' > Enter

For the first sector take the start number of /dev/vda2 for the last sector use the provided value which should be the
full disk size

sudo shutdown -r now

sudo resize2fs /dev/vda
```

## Credits

- [Using QEMU to emulate a Raspberry Pi](https://blog.agchapman.com/using-qemu-to-emulate-a-raspberry-pi/)
- [qemu-rpi-kernel](https://github.com/dhruvvyas90/qemu-rpi-kernel)
- My Blog post about this project [here](https://ncrmro.com/posts/gondola-ansible-playbook-for-emulating-raspberry-pi-os-with-kvm)
