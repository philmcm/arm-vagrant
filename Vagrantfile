# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

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
      git
    git clone https://github.com/signal11/hidapi.git
    cd hidapi
    ./bootstrap
    ./configure
    make
    sudo make install
    sudo ldconfig
    cd ..
#    rm -rf hidapi
    git clone git://repo.or.cz/openocd.git
    cd openocd
    ./bootstrap
    ./configure --enable-cmsis-dap --enable-jlink
    make
    sudo make install
    cd ..
#    rm -rf openocd
    echo 'SUBSYSTEM=="usb|hidraw", ATTRS{idVendor}=="0d28", ATTRS{idProduct}=="0204", MODE="0666"' | sudo tee -a /etc/udev/rules.d/50-OpenOCDProg.rules
    echo 'SUBSYSTEM=="usb|hidraw", ATTRS{idVendor}=="1fc9", ATTRS{idProduct}=="0090", MODE="0666"' | sudo tee -a /etc/udev/rules.d/50-OpenOCDProg.rules
    echo 'SUBSYSTEM=="usb|hidraw", ATTRS{idVendor}=="1366", ATTRS{idProduct}=="0101", MODE="0666"' | sudo tee -a /etc/udev/rules.d/50-OpenOCDProg.rules
    sudo udevadm control --reload-rules
    sudo udevadm trigger
  SHELL
end
