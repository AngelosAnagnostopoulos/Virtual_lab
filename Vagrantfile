Vagrant.configure("2") do |config|
	
	config.ssh.insert_key = false
	config.vm.provider :virtualbox do |vb|
		vb.memory = 512
		vb.cpus = 1
	end

	N = 1
	(0..N).each do |i|
		config.vm.define "server_#{i}" do |node|
			node.vm.hostname = "web#{i}"
			node.vm.box = "centos/7"
			node.vm.network :private_network, ip: "192.168.56.#{10+i}", virtualbox_intnet: true
			node.vm.network :forwarded_port, host: 8080+i, guest: 80
			
			# For manual ipv4 address:
			# config.vm.network "public_network", auto_config: false
			# config.vm.provision "shell",
			# run: "always"
			# inline: "ifconfig eth1 192.168.0.17 netmask 255.255.255.0 up"
		
			node.vm.provision "ansible" do |ansible|
				ansible.verbose = "v"
				ansible.playbook = "web_playbook.yml"
			end

		end
	end

	config.vm.define "database" do |db|
		db.vm.hostname = "redis"
		db.vm.box = "centos/7"
		db.vm.network :private_network, ip: "192.168.56.100", virtualbox_intnet: true
		db.vm.network :forwarded_port, host: 12345, guest: 6379
		db.vm.provision "ansible" do |ansible|
			ansible.verbose = "v"
			ansible.playbook = "redis_playbook.yml"
		end
	end
end
