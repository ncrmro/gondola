# Gondola

## Description
This project should provide an ansible playbook that provisions a
emulated raspberry pi virtual machine on the local KVM machine.

## Installation

## Usage

`sh main.sh`

At this point you should be able to ping your pi

`ping raspberrypi.local`

You might drop into the guest and enable openssh-server username is `pi` password is `raspberry`

`sudo raspi-config`

Then enable the openssh-server

And then from your host machine

``ssh-copy-id -i ~/.ssh/id_ed25519.pub pi@raspberrypi.local``

Now you should be able ssh in.

`ssh pi@raspberrypi.local`


## Roadmap

* [x] Ansible Setup with Connection to Specified Host - Hello World
    * [x] Make sure KVM NAT network is accessible with DNS
* [x] Download Raspberry Pi Image
* [x] Download the necessary kernals
* [ ] Asnible Jinja Templete Libvirt Guest XML
* [x] Initial provision of SSH key and any required packages
    * [ ] Clone the VM this guest can be developed against.
    * [ ] Dump Image to Be burned to USB
* [ ] Develop Image (our Firmware)
* [ ] Create a Firmware Release (qcow2 back to bootable image)
