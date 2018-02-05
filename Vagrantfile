# Boxes
# - ubuntu/trusty64 (14.04)
# - ubuntu/xenial64 (16.04)
# - cmiles/gentoo-amd64-minimal
# - debian/jessie64
# - debian/wheezy64

#vms = {
#  "gentoo" => {
#    box => "cmiles/gentoo-amd64-minimal",
#    cpus => 4,
#    memory => 512,
#    ip => "192.168.100.2",
#    hostname => "gentoo.example.com",
#    aliases => "",
#    ports => [],
#    shares => [],
#  }
#}

Vagrant.configure("2") do |config|

  vms.each_pair do |vm_name, vm_config|
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.manage_guest = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true

    config.vm.define vm_name do |config|
      config.vm.box = vm_config[:box]
      config.vm.hostname = vm_config[:hostname]
      config.vm.network :private_network, ip: vm_config[:ip]
      config.hostmanager.aliases = vm_config[:aliases]

      vm_config[:shares].each { |share|
          config.vm.synced_folder share[:local], share[:remote]
      } # vm_config[:shares].each

      vm_config[:ports].each { |port_forwarding|
          config.vm.network :forwarded_port,
            host:  port_forwarding[:host_port],
            guest: port_forwarding[:guest_port],
            id:    port_forwarding[:id]
      } # vm_config[:ports].each

      config.vm.provider "virtualbox" do |vb|
        vb.name = vm_name
        vb.memory = vm_config[:memory]
        vb.cpus = vm_config[:cpus]
      end # :virtualbox

      config.vm.provision "ansible" do |ansible|
        ansible.groups = inventory_groups
        ansible.playbook = "playbook.yml"
        ansible.become = true

        #ansible.become_user = "root"
        #ansible.become_ask_pass = true
        #ansible.limit = "webservers" # connect to ALL the vagrants!
        #ansible.ask_vault_pass = false # Change to true when using vaults.
        #ansible.host_key_checking = false
        #ansible.extra_vars = "" # string or hash
        #ansible.skip_tags = []
        #ansible.start_at_task = "Ensure php-fpm is started and enabled at boot (if configured)."
        #ansible.tags = []

      end # :ansible

    end # config.vm.define
  end # vms.each_pair

end # Vagrant.config
