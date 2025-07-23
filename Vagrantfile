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

  def configurar_ansible_local(vm, inventario)
    vm.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/vagrant/ansible/playbooks/site.yml"
      ansible.inventory_path = "/vagrant/#{inventario}"
      ansible.compatibility_mode = "2.0"
    end
  end

  # ------------------- VM ARQ (DHCP SERVER) -------------------
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.maricleia.devops"

    arq.vm.network "private_network",
      ip: "192.168.56.132",
      virtualbox__intnet: "rede_local",
      auto_config: true,
      adapter: 2

    # Discos extras para LVM
    arq.vm.disk :disk, size: "10GB", name: "disk_a"
    arq.vm.disk :disk, size: "10GB", name: "disk_b"
    arq.vm.disk :disk, size: "10GB", name: "disk_c"

    arq.vm.provider "virtualbox" do |vb|
      tune_vm(vb, 512)
    end

    configurar_ansible_local(arq, "hosts-arq.ini")
  end

  # ------------------- VM DB (IP FIXO: .155) -------------------
  config.vm.define "db" do |db|
    db.vm.hostname = "db.maricleia.devops"

    db.vm.network "private_network",
      ip: "192.168.56.155",
      virtualbox__intnet: "rede_local",
      auto_config: true,
      adapter: 2

    db.vm.provider "virtualbox" do |vb|
      tune_vm(vb, 512)
    end

    configurar_ansible_local(db, "hosts-db.ini")
  end

  # ------------------- VM APP (IP FIXO: .135) -------------------
  config.vm.define "app" do |app|
    app.vm.hostname = "app.maricleia.devops"

    app.vm.network "private_network",
      ip: "192.168.56.135",
      virtualbox__intnet: "rede_local",
      auto_config: true,
      adapter: 2

    app.vm.provider "virtualbox" do |vb|
      tune_vm(vb, 512)
    end

    configurar_ansible_local(app, "hosts-app.ini")
  end

  # ------------------- VM CLI (IP por DHCP) -------------------
  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.maricleia.devops"

    cli.vm.network "private_network",
      type: "dhcp",
      virtualbox__intnet: "rede_local",
      auto_config: true,
      adapter: 2

    cli.vm.provider "virtualbox" do |vb|
      tune_vm(vb, 1024)
    end

    configurar_ansible_local(cli, "hosts-cli.ini")
  end
end
