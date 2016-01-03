Vagrant.configure("2") do |config|

  # docker webservers
  config.vm.define "nginx" do |nginx|
      nginx.vm.box = 'ubuntu/trusty64'
      nginx.vm.hostname = "hadocker-nginx"

      nginx.vm.network :private_network, ip: "192.168.111.225"

      nginx.vm.provision "ansible" do |ansible|
        ansible.inventory_path  = "ansible/inventory"
        ansible.limit           = 'nginx'
        ansible.playbook        = "ansible/setup.yml"
        ansible.extra_vars      = { ansible_ssh_user: 'vagrant' }
      end
  end

  # haproxy
  config.vm.define "haproxy" do |haproxy|
      haproxy.vm.box = 'ubuntu/trusty64'
      haproxy.vm.hostname = "hadocker-haproxy"

      haproxy.vm.network :private_network, ip: "192.168.111.224"

      haproxy.vm.provision "ansible" do |ansible|
        ansible.inventory_path  = "ansible/inventory"
        ansible.limit           = 'haproxy'
        ansible.playbook        = "ansible/setup.yml"
        ansible.extra_vars      = { ansible_ssh_user: 'vagrant' }
      end
  end

end
