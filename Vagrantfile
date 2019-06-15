# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

# Capture USB devices on startup using virtualbox USB filters.
# Note: the LPC-Link2 with J-Link software will enumerate as Segger.

  config.vm.provider "virtualbox" do |vb|
    vb.customize ['modifyvm', :id, '--usb', 'on']
    vb.customize ['modifyvm', :id, '--usbehci', 'on']
    vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'DAPLink CMSIS-DAP', '--vendorid', '0x0D28', '--productid', '0x0204']
    vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'LPC-Link 2', '--vendorid', '0x1fc9', '--productid', '0x0090']
    vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'SEGGER J-Link PLUS', '--vendorid', '0x1366', '--productid', '0x0101']
  end

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install -y \\
      gcc \\
      g++ \\
      make \\
      libtool \\
      pkg-config \\
      autoconf \\
      libusb-1.0 \\
      libudev-dev \\
      linux-image-extra-virtual \\
      git \\
      cmake

# Download and build HIDAPI, required for CMSIS-DAP support on OpenOCD.

    git clone https://github.com/signal11/hidapi.git
    cd hidapi
    ./bootstrap
    ./configure
    make
    sudo make install
    sudo ldconfig
    cd ..
#    rm -rf hidapi

# Download and build OpenOCD.

    git clone git://repo.or.cz/openocd.git
    cd openocd
    ./bootstrap
    ./configure --enable-cmsis-dap --enable-jlink --enable-stlink
    make
    sudo make install
    cd ..
#    rm -rf openocd

# Add rules for UDEVs, these allow the vagrant user to run OpenOCD without being superuser.
# Note: only CMSIS-DAP requires HID, others require only USB.

    echo 'SUBSYSTEM=="usb|hidraw", ATTRS{idVendor}=="0d28", ATTRS{idProduct}=="0204", MODE="0664", GROUP="vagrant"' | sudo tee -a /etc/udev/rules.d/50-OpenOCDProg.rules
    echo 'SUBSYSTEM=="usb|hidraw", ATTRS{idVendor}=="1fc9", ATTRS{idProduct}=="0090", MODE="0664", GROUP="vagrant"' | sudo tee -a /etc/udev/rules.d/50-OpenOCDProg.rules
    echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="1366", ATTRS{idProduct}=="0101", MODE="0664", GROUP="vagrant"' | sudo tee -a /etc/udev/rules.d/50-OpenOCDProg.rules
    sudo udevadm control --reload-rules
    sudo udevadm trigger

# Install and setup ARM GCC.

    wget -q https://developer.arm.com/-/media/Files/downloads/gnu-rm/8-2018q4/gcc-arm-none-eabi-8-2018-q4-major-linux.tar.bz2
    tar xjf gcc-arm-none-eabi-8-2018-q4-major-linux.tar.bz2
    echo 'export ARMGCC_DIR=~/gcc-arm-none-eabi-8-2018-q4-major' | sudo tee -a .bashrc

  SHELL
end
