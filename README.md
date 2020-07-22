# Gondola

## Description

This project should provide an ansible playbook that provisions a
emulated raspberry pi virtual machine on the local KVM machine.

A quick over view, this project gets the following files and puts them in your libvirt images folder and then creates the 
kvm guest and creates a clone. From there you can clone that initial image for development related tasks.

| File                                     | Purpose                                                                            |
| ---------------------------------------- | ---------------------------------------------------------------------------------- |
| 2020-05-27-raspios-buster-lite-armhf.img | The [image](https://www.raspberrypi.org/downloads/) containng the operating system |
| kernel-qemu-4.19.50-buster               | The [kernal](<https://en.wikipedia.org/wiki/Kernel_(operating_system)>)            |
| versatile-pb-buster.dtb                  | The [device tree blob](https://superuser.com/a/736988)                             |

## Installation

Make sure you have `libvirt-dev` and `qemu-system-arm` installed as well.

`pip install -r requirements.txt`

## Usage

`sh main.sh`

At this point you should be able to ping your pi

`ping raspberrypi.local`

You might drop into the guest and enable openssh-server username is `pi` password is `raspberry`

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

`virt-clone --original pi --auto-clone -d my_dev_pi`

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

## Credits

- [Using QEMU to emulate a Raspberry Pi](https://blog.agchapman.com/using-qemu-to-emulate-a-raspberry-pi/)
- [qemu-rpi-kernel](https://github.com/dhruvvyas90/qemu-rpi-kernel)
