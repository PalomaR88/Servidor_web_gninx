# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define :nginx do |nginx|
    nginx.vm.box = "debian/buster64"
    nginx.vm.hostname = "servidornginx"
    nginx.vm.network :public_network,:bridge=>"enp2s0"
  end
end
