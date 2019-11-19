# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define :nginx do |nginx|
    nginx.vm.box = "debian/buster64"
    nginx.vm.hostname = "servidornginx"
    nginx.vm.network :public_network,:bridge=>"wlp1s0"
    nginx.vm.network :private_network, ip: "192.168.100.1", virtualbox__intnet:"mired1"
  end

  config.vm.define :cliente do |cliente|
    cliente.vm.box = "debian/buster64"
    cliente.vm.hostname = "cliente"
    cliente.vm.network :private_network, ip:"192.168.100.2", virtualbox__intnet:"mired1"

  end
end
