VAGRANT_BOX        = "bento/ubuntu-24.04"
CTRL_IP            = "192.168.56.100"
WORKER_IP_BASE     = "192.168.56."
WORKER_COUNT       = 2      

CTRL_CPUS          = 1
CTRL_MEMORY_MB     = 4096     

WORKER_CPUS        = 2
WORKER_MEMORY_MB   = 6144      

Vagrant.configure("2") do |config|
  config.vm.box = VAGRANT_BOX
  config.vm.synced_folder ".", "/vagrant"

  config.vm.define "ctrl" do |node|
    node.vm.hostname = "ctrl"
    node.vm.network "private_network", ip: CTRL_IP

    node.vm.provider "virtualbox" do |vb|
      vb.name   = "k8s-ctrl"
      vb.cpus   = CTRL_CPUS
      vb.memory = CTRL_MEMORY_MB
    end

    node.vm.provision "ansible_local" do |ansible|
      ansible.install = true
      ansible.become  = true
      ansible.playbook = "/vagrant/general.yaml"
      ansible.extra_vars = {
        worker_count: WORKER_COUNT,
        ctrl_ip: CTRL_IP,
        worker_ip_base: WORKER_IP_BASE
      }
    end

    node.vm.provision "ansible_local" do |ansible|
      ansible.install = true
      ansible.become  = true
      ansible.playbook = "/vagrant/ctrl.yaml"
    end
  end

  (1..WORKER_COUNT).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.hostname = "node-#{i}"
      worker_ip_last_octet = 100 + i
      node.vm.network "private_network", ip: "#{WORKER_IP_BASE}#{worker_ip_last_octet}"

      node.vm.provider "virtualbox" do |vb|
        vb.name   = "k8s-node-#{i}"
        vb.cpus   = WORKER_CPUS
        vb.memory = WORKER_MEMORY_MB
      end

      node.vm.provision "ansible_local" do |ansible|
        ansible.install = true
        ansible.become  = true
        ansible.playbook = "/vagrant/general.yaml"
        ansible.extra_vars = {
          worker_count: WORKER_COUNT,
          ctrl_ip: CTRL_IP,
          worker_ip_base: WORKER_IP_BASE
        }
      end

      node.vm.provision "ansible_local" do |ansible|
        ansible.install = true
        ansible.become  = true
        ansible.playbook = "/vagrant/node.yaml"
        ansible.extra_vars = {
          ctrl_ip: CTRL_IP
        }
      end
    end
  end
end
