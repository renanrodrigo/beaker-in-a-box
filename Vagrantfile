# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos-6"
  config.vm.box_url = "https://download.gluster.org/pub/gluster/purpleidea/vagrant/centos-6/centos-6.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  #config.vm.hostname
  #config.vm.network
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
  # create beaker server and testing systems
  test_systems = [
    { :hostname => 'beaker-test-vm1', :ip => '192.168.120.105', :mac => '52:54:00:c6:71:8e' },
    { :hostname => 'beaker-test-vm2', :ip => '192.168.120.106', :mac => '52:54:00:c6:71:8f' },  
    { :hostname => 'beaker-test-vm3', :ip => '192.168.120.107', :mac => '52:54:00:c6:71:90' },
  ]
  test_systems.each do |system|
    config.vm.define system[:hostname] do |machine|
      machine.vm.host_name = system[:hostname]
      machine.vm.network "private_network", libvirt__network_name: "beaker",
                         ip: system[:name], mac: system[:mac], model_type: "virtio"
      machine.vm.provider :libvirt do |v|
        v.driver = 'kvm'
        v.memory = 2048
        v.graphics_port = '-1'
      end
    end
  end
  config.vm.define 'beaker-server-lc' do |server|
    server.vm.hostname = 'beaker-server-lc.beaker'
    server.ssh.username = 'root'
    server.ssh.password = 'vagrant'
    server.vm.network "private_network", libvirt__network_name: "beaker",
                      ip: "192.168.120.104", mac: "52:54:00:c6:73:4f"
    server.vm.provider :libvirt do |v|
        v.driver = 'kvm'
        v.memory = 2048
        v.cpus = 2
    end
    server.vm.provision "ansible" do |ansible|
      ansible.playbook="beaker-setup/site.yml"
    end
  end

end