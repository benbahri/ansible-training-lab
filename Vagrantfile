#  -*-  mode:  ruby -*-
# vi: set ft=ruby  :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION)  do  |config|
  # General Vagrant VM   configuration.
  config.vm.box  =  "centos/7"
  config.ssh.insert_key  =  false
  config.vm.synced_folder  ".",  "/vagrant"
  config.vm.provider  :virtualbox  do  |v|
  	v.memory  =  1024
  	v.linked_clone  =  true
	end

  #  ControlMaster
  config.vm.define  "master"  do  |app|
    app.vm.hostname  =  "master.dev"
    app.vm.network  :private_network,  ip:  "192.168.60.100"
    app.vm.provider  :virtualbox  do  |v|
    	v.memory  =  4096
    end
    app.vm.provision "file", source: "./id_rsa", destination: "/home/vagrant/.ssh/"
    app.vm.provision :shell, path: 'setup.sh'
  end

	#  Application  server 1.
	config.vm.define  "app1"  do  |app|
  	app.vm.hostname  =  "app1.dev"
  	app.vm.network  :private_network,  ip:  "192.168.60.4"
    app.vm.provision "shell", inline: $script_inject_pk
	end

	#  Application  server 2.
	config.vm.define  "app2"  do  |app|
  	app.vm.hostname  =  "app2.dev"
  	app.vm.network  :private_network,  ip:  "192.168.60.5"
    app.vm.provision "shell", inline: $script_inject_pk
	end

	#  Database  server.
	config.vm.define  "db"  do  |db|
  	db.vm.hostname  =  "db.dev"
  	db.vm.network  :private_network,  ip:  "192.168.60.6"
    db.vm.provision "shell", inline: $script_inject_pk
	end
end

$script_inject_pk =<<-'SCRIPT'
  cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
SCRIPT
