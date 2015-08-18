# -*- mode: ruby -*-
# vi: set ft=ruby et ts=4 sw=4 sts=0:
Vagrant.configure(2) do |config|
    config.vm.box = "terrywang/archlinux"
    config.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--uart1", "0x3f8", "4"]
        vb.customize ["modifyvm", :id, "--uartmode1", "tcpserver", "9999"]
    end
end
