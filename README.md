# Vagrant Environment for Arm development

A Vagrant/VirtualBox environment including OpenOCD, ARM GCC, etc. primarily focused on probes supporting CMSIS-DAP for NXP Kinetis builds.  However, this should be extendable to many other Arm Cortex-M based applications.

Preliminary testing has been performed with the following debug probes:
- Segger J-Link Plus
- NXP Kinetis FRDM-K28F board (running CMSIS-DAP firmware)
- LPC-Link2 using J-Link Plus firmware (currently, CMSIS-DAP is not working)

I have tested the environment under Pop!OS 18.04 LTS, my personal Linux desktop of choice.  It should run on any other system supporting VirtualBox and Vagrant.

STM and ST-Link support is in progress.

Provided "as is".  Use at own risk.  Please raise any issues here in GitHub and thank you for your support.

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
