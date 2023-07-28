Vagrant.configure("2") do |config|
	
	# config.ssh.insert_key = false
	config.vm.provider :virtualbox do |vb|
		vb.memory = 512
		vb.cpus = 1
	end

	N = 1
	(0..N).each do |i|
	config.vm.define "server#{i}" do |node|
		node.vm.hostname = "web#{i}"
		node.vm.box = "centos/7"
		node.vm.network "public_network", ip: "192.168.56.#{10+i}"
		node.vm.provision "ansible" do |ansible|
			ansible.verbose = "v"
			ansible.playbook = "web_playbook.yml"
			end
		end
  	end

	config.vm.define "database" do |db|
		db.vm.hostname = "sql"
		db.vm.box = "centos/7"
		db.vm.network "public_network", ip: "192.168.56.100"
		db.vm.provision "ansible" do |ansible|
			ansible.verbose = "v"
			ansible.playbook = "sql_playbook.yml"
		end
	end

end
