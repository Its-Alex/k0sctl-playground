# -*- mode: ruby -*-
# vi: set ft=ruby :

cluster = {
  "cluster-001" => { :ip => "192.168.56.10", :cpus => 2, :mem => 2048 },
  "cluster-002" => { :ip => "192.168.56.11", :cpus => 2, :mem => 2048 }
}

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"

  cluster.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.box = "bento/ubuntu-22.04"
      
      cfg.vm.synced_folder ".", "/vagrant", disabled: true
      cfg.vm.provision "file", source: "./ssh_key/", destination: "/home/vagrant/"
      
      cfg.vm.provider :virtualbox do |vb, override|
        override.vm.hostname = hostname
        vb.name = hostname

        # Create a private network, which allows host-only access to the machine
        # using a specific IP.
        override.vm.network :private_network, ip: "#{info[:ip]}"

        vb.cpus = info[:cpus]
        vb.memory = info[:mem]
      end

      cfg.vm.provision "shell", inline: <<-SHELL
        cat /home/vagrant/ssh_key/vagrant_dev.pub >> /home/vagrant/.ssh/authorized_keys
      SHELL
    end
  end

end
