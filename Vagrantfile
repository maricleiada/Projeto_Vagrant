# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.ssh.insert_key = false

  def tune_vm(vb, ram = 512)
    vb.memory = ram
    vb.linked_clone = true
    vb.check_guest_additions = false
  end

  # ------------------- VM ARQ (Servidor DHCP) -------------------
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.maricleia.devops"

    # eth1 - IP fixo, rede interna 'rede_local'
    arq.vm.network "private_network",
      ip: "192.168.56.132",
      virtualbox__intnet: "rede_local",
      auto_config: true,
      adapter: 2

   

    # Discos extras
    arq.vm.disk :disk, size: "10GB", name: "disk_a"
    arq.vm.disk :disk, size: "10GB", name: "disk_b"
    arq.vm.disk :disk, size: "10GB", name: "disk_c"

    arq.vm.provider "virtualbox" do |vb|
      tune_vm(vb, 512)
    end

    # Ansible local para configurar DHCP
    arq.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/vagrant/ansible/playbooks/site.yml"
      ansible.inventory_path = "/vagrant/hosts.ini"
      ansible.compatibility_mode = "2.0"
    end
  end

  # ------------------- VMs Clientes com DHCP via eth1 -------------------
  ["db", "app", "cli"].each do |name|
    config.vm.define name do |vm|
      vm.vm.hostname = "#{name}.maricleia.devops"

      # eth1 - DHCP pela rede interna 'rede_local'
      vm.vm.network "private_network",
        type: "dhcp",
        virtualbox__intnet: "rede_local",
        auto_config: true,
        adapter: 2

      vm.vm.provider "virtualbox" do |vb|
        ram = (name == "cli") ? 1024 : 512
        tune_vm(vb, ram)
      end
    end
  end
end
