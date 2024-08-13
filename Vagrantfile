Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/jammy64'
  config.ssh.insert_key = true

  config.vm.provider "virtualbox" do |v|
#    v.linked_clone = true
    v.memory = 4096
    v.cpus = 2
  end

  # edgex runtime node
  config.vm.define 'main' do |machine|
    machine.vm.hostname = 'main'
    machine.vm.network 'forwarded_port', id: 'ssh', host: 2221, guest: 22
    machine.vm.network 'forwarded_port', host: 3000, guest: 3000, protocol: "tcp"
    machine.vm.network 'private_network', virtualbox__intnet: 'cluster', ip: '10.0.0.10'
  end

  config.vm.define 'target' do |machine|
    machine.vm.hostname = 'target'
    machine.vm.network 'forwarded_port', id: 'ssh', host: 2222, guest: 22
    machine.vm.network 'private_network', virtualbox__intnet: 'cluster', ip: '10.0.0.20'
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "preflight.yml"
  end
end
