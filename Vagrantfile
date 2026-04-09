Vagrant.configure("2") do |config|
  config.vm.box = "centos/stream9"
  config.vm.box_version = "20260406.0"
  config.vm.hostname = "centos-ansible"
  config.vm.network "forwarded_port", guest: 3000, host: 3000, host_ip: "127.0.0.1", auto_correct: true

  config.vm.provider "virtualbox" do |vb|
    vb.name = "centos-ansible"
    vb.memory = 4096
    vb.cpus = 2
  end

  config.vm.provision "file", source: "ansible", destination: "/home/vagrant/ansible"

  config.vm.provision "shell", inline: <<-SHELL
    dnf install -y ansible-core
    chown -R vagrant:vagrant /home/vagrant/ansible
  SHELL

  config.vm.provision "ansible_local" do |ansible|
    ansible.provisioning_path = "/home/vagrant"
    ansible.playbook = "ansible/site.yml"
    ansible.install = false
    ansible.compatibility_mode = "2.0"
    ansible.limit = "all"
    ansible.verbose = "v"
  end
end
