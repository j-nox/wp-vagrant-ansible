nodes = [
  { :hostname => 'wp-server', :ip => '192.168.56.23', :ram => 1024, :guestport => 3306, :hostport => 18306 },
  { :hostname => 'db-server', :ip => '192.168.56.24', :ram => 1024, :guestport => 3306, :hostport => 19306 }
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = 'ubuntu/xenial64'
      nodeconfig.vm.hostname = node[:hostname] + ".local"
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.network "forwarded_port", guest: node[:guestport], host: node[:hostport]

      memory = node[:ram] ? node[:ram] : 1024;

      nodeconfig.vm.provider :virtualbox do |vb|
        vb.customize [ "modifyvm", :id, "--memory", memory.to_s ]
        vb.customize [ "modifyvm", :id, "--audio", "none" ]
      end

    end

  end

  config.vm.define "wp-server" do |wp|
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/wp.yml"
      ansible.groups = {
        "web-servers" => ["wp-server"]
      }
    end
  end

  config.vm.define "wp-server" do |wp|
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbooks/db.yml"
      ansible.groups = {
        "db-servers" => ["db-server"]
      }
    end
  end

end
