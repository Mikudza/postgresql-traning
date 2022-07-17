# -*- mode: ruby -*-
# vi: set ft=ruby :
nodes = [
  { :hostname => 'server', :ip => '192.168.56.5', :memory => 1024, :cpu => 1, :boxname => "generic/ubuntu2004" },
  { :hostname => 'client', :ip => '192.168.56.4', :memory => 1024, :cpu => 1, :boxname => "generic/ubuntu2004" },
]
Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.box_check_update = false
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = node [:boxname]
      nodeconfig.vm.hostname = node [:hostname]
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.provider :virtualbox do |vb|
          vb.memory = node[:memory]
          vb.cpus = node[:cpu]
      end

      nodeconfig.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install net-tools
        apt install postgresql postgresql-contrib -y
        systemctl start postgresql@12-main
        sudo -u postgres createuser test_user -i -s
        sudo -u postgres createdb test_user
        adduser --quiet --disabled-password --no-create-home --gecos test_user test_user
      SHELL

    end
  end
end