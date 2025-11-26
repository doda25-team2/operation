VAGRANT_BOX        = "bento/ubuntu-24.04"
CTRL_IP            = "192.168.56.100"
WORKER_IP_BASE     = "192.168.56."
WORKER_COUNT       = 1        

CTRL_CPUS          = 1
CTRL_MEMORY_MB     = 4096     

WORKER_CPUS        = 2
WORKER_MEMORY_MB   = 6144      

Vagrant.configure("2") do |config|
  config.vm.box = VAGRANT_BOX

  # optional: disable synced folder to avoid weirdness
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # ---- controller VM ----
  config.vm.define "ctrl" do |node|
    node.vm.hostname = "ctrl"
    node.vm.network "private_network", ip: CTRL_IP

    node.vm.provider "virtualbox" do |vb|
      vb.name   = "k8s-ctrl"
      vb.cpus   = CTRL_CPUS
      vb.memory = CTRL_MEMORY_MB
    end
  end

  # ---- worker VMs ----
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
    end
  end
  # general.yaml
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "general.yaml"
    ansible.become = true
    ansible.install = true
    ansible.extra_vars = { worker_count: WORKER_COUNT }
  end

  # ctrl.yaml
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ctrl.yaml"
    ansible.limit = "ctrl"
    ansible.become = true
    ansible.install = true

  end

  # node.yaml
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "node.yaml"
    ansible.limit = "node-*"
    ansible.become = true
    ansible.install = true
  end
end
