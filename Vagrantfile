# -*- mode: ruby -*-
# vi: set ft=ruby :

module OS
	def OS.windows?
		(/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
	end

	def OS.mac?
		(/darwin/ =~ RUBY_PLATFORM) != nil
	end

	def OS.unix?
		!OS.windows?
	end

	def OS.linux?
		OS.unix? and not OS.mac?
	end
end

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "debian/jessie64"
  config.ssh.forward_agent = true

  config.vm.define :web, primary:true do |main|
    main.vm.network :private_network, ip: "192.33.88.100"

    if OS.windows?
        main.vm.synced_folder ".", "/vagrant", type: "nfs", :mount_options => ["dmode=777,fmode=777"]
    else
        main.vm.synced_folder ".", "/vagrant", type: "nfs", :mount_options => ['nolock,vers=3,udp,noatime,actimeo=1']
    end

    main.vm.hostname = "symfony"

    #Install ansible inside box
    config.vm.provision "shell", inline: <<-SHELL
        sudo echo "deb http://httpredir.debian.org/debian jessie-backports main contrib non-free" > /etc/apt/sources.list.d/jessie-backports.list
        sudo apt-get update
        sudo apt-get -t jessie-backports install ansible -y
    SHELL

    main.vm.provider :virtualbox do |vb|
        vb.customize [
            "modifyvm", :id,
            "--memory", 1024,
        ]
    end

  end
end