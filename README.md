# Gondola

## Description
This project should provide an ansible playbook that provisions a
emulated raspberry pi virtual machine on the local KVM machine.

## Installation

## Usage

`sh main.sh`

`cp temp/2020-05-27-raspios-buster-lite-armhf.img `

## Roadmap

* [x] Ansible Setup with Connection to Specified Host - Hello World
    * [x] Make sure KVM NAT network is accessible with DNS
* [x] Download Raspberry Pi Image
* [x] Download the necessary kernals
* [ ] Asnible Jinja Templete Libvirt Guest XML
* [ ] Initial provision of SSH key and any required packages
    * [ ] Clone the VM this guest can be developed against.
    * [ ] Dump Image to Be burned to USB
* [ ] Develop Image (our Firmware)
* [ ] Create a Firmware Release
