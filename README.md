# Vagrant Environment for Arm development

A Vagrant/VirtualBox environment including OpenOCD, ARM GCC, etc.

Preliminary testing has been performed with the following debug probes:
- Segger J-Link Plus
- NXP Kinetis FRDM-K28F board (running CMSIS-DAP firmware)
- LPC-Link2 using J-Link Plus firmware (currently, CMSIS-DAP is not working)

STM and ST-Link support is in progress.

Provided "as is".  Use at own risk.

## Installation Prerequisites

- VirtualBox on your preferred platform.
- Vagrant.

Vagrant will take care of the rest!

## Installation Instructions

```
git clone https://github.com/philmcm/arm-vagrant.git
cd arm-vagrant
vagrant up
```

Once installation is complete, `vagrant ssh` will get you into the environment.
Use `openocd`, `cmake`, and anything else you need.

## Thank You

This work is based on Emilie Gillet's mutable-dev-environment (https://github.com/pichenettes/mutable-dev-environment.git).

When I had issues I consulted Emilie's work and often got the answer I needed.  Thanks Emilie!
