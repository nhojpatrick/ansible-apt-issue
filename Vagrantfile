# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

	# fix "stdin: is not a tty"
	config.vm.provision "fix-no-tty", type: "shell" do |s|
		s.privileged = false
		s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
	end

	config.vm.provider :virtualbox do |vb|
		vb.gui = false
		vb.customize ["modifyvm", :id, "--memory", "384"]
	end

#	config.vm.box = "ubuntu/xenial64" # 16.04
#	config.vm.box = "ubuntu/zesty64" # 17.04
	config.vm.box = "ubuntu/artful64" # 17.10

	config.vm.boot_timeout = 600

	config.vm.define "ansible-apt-module", autostart: false do |inst|
		inst.vm.hostname = "ansible-apt-module"
		inst.vm.provision :ansible_local do |ansible|
			ansible.playbook = "provisioning/ansible-apt-module.yaml"
			#ansible.verbose = true
			ansible.verbose = "-vvvv"
		end
	end

	config.vm.define "ansible-shell-module", autostart: false do |inst|
		inst.vm.hostname = "ansible-shell-module"
		inst.vm.provision :ansible_local do |ansible|
			ansible.playbook = "provisioning/ansible-shell-module.yaml"
			ansible.verbose = true
		end
	end

	config.vm.define "shell", autostart: false do |inst|
		inst.vm.hostname = "shell"
		inst.vm.provision :ansible_local do |ansible|
			ansible.playbook = "provisioning/shell.yaml"
			ansible.verbose = true
		end
		inst.vm.provision "apt-install-mysql-server", type: "shell" do |s|
			s.privileged = false
			s.inline = "sudo apt install mysql-server --yes"
		end
	end

end
