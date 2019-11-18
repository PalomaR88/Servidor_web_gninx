# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define :ginx do |ginx|
    ginx.vm.box = "debian/buster64"
    ginx.vm.hostname = "servidorginx"
    ginx.vm.network :public_network,:bridge=>"enp2s0"
  end
end
