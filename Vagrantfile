VAGRANT_BOX         = "bento/ubuntu-24.04"
VAGRANT_BOX_VERSION = "202510.26.0"
CTRL_IP             = "192.168.56.100"
WORKER_IP_BASE      = "192.168.56."
WORKER_COUNT        = 2

CTRL_CPUS           = 2
CTRL_MEMORY_MB      = 4096

WORKER_CPUS         = 2
WORKER_MEMORY_MB    = 6144

# Shared storage for Kubernetes hostPath volumes
SHARED_FOLDER       = "./shared"

# Optional: custom SSH host and boot timeout values
SSH_HOST = ENV["SSH_HOST"]
BOOT_TIMEOUT = ENV["BOOT_TIMEOUT"]

Vagrant.configure("2") do |config|
  config.vm.box = VAGRANT_BOX
  config.vm.box_version = VAGRANT_BOX_VERSION
  config.vm.synced_folder ".", "/vagrant"

  config.ssh.host = SSH_HOST if SSH_HOST
  config.vm.boot_timeout = BOOT_TIMEOUT.to_i if BOOT_TIMEOUT

  # Shared folder for cross-VM and cross-Pod persistent storage
  # This enables Kubernetes hostPath volumes to share data across all nodes
  config.vm.synced_folder SHARED_FOLDER, "/mnt/shared", create: true

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "./ansible/general.yaml"
    ansible.extra_vars = {
      ctrl_ip: CTRL_IP,
      worker_count: WORKER_COUNT,
      worker_ip_base: WORKER_IP_BASE
    }
  end

  config.vm.define "ctrl" do |node|
    node.vm.hostname = "ctrl"
    node.vm.network "private_network", ip: CTRL_IP

    node.vm.provider "virtualbox" do |vb|
      vb.name   = "k8s-ctrl"
      vb.cpus   = CTRL_CPUS
      vb.memory = CTRL_MEMORY_MB
    end

    node.vm.provision "ansible" do |ansible|
      ansible.playbook = "./ansible/ctrl.yaml"
      ansible.extra_vars = {
        ctrl_ip: CTRL_IP
      }
    end
  end

  # Worker nodes
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

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "./ansible/node.yaml"
        ansible.extra_vars = {
          ctrl_hostname: "ctrl"
        }
      end

      # Run finalization only after the last worker node is provisioned
      # Target the ctrl node from node-2 to ensure all nodes are up
      if i == WORKER_COUNT
        node.vm.provision "ansible" do |ansible|
          ansible.playbook = "./ansible/finalization.yaml"
          ansible.limit = "ctrl"
          ansible.extra_vars = {
            ctrl_ip: CTRL_IP,
            worker_count: WORKER_COUNT,
            worker_ip_base: WORKER_IP_BASE
          }
        end
      end
    end
  end
end
