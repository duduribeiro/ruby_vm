Vagrant.configure('2') do |config|
  #config.vm.box = 'ubuntu/xenial64'
  config.vm.box = 'bento/ubuntu-16.04'

  config.vm.network 'private_network', ip: '11.0.1.6'

  `vagrant plugin install vagrant-omnibus` unless Vagrant.has_plugin?('vagrant-omnibus')
  config.omnibus.chef_version = '12.5.1' if Vagrant.has_plugin?('vagrant-omnibus')

  #config.vbguest.auto_update = false if Vagrant.has_plugin?('vagrant-vbguest')

  config.vm.provider 'virtualbox' do |v|
    v.memory = 2048
    v.cpus = 4
    #v.gui = true
    v.customize ["modifyvm", :id, "--cableconnected1", "on"]
  end

  config.vm.network 'forwarded_port', guest: 3000, host: 3000 # rails

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = 'cookbooks'
    chef.roles_path = 'roles'
    chef.provisioning_path = '/tmp/vagrant-chef'
    chef.add_role('rvm')
    chef.add_role('postgres')
    chef.add_role('redis')

    chef.add_recipe('rvm')
    chef.json = {
      # rvm: {
      #   default_ruby: 'ruby-2.3.1-railsexpress'
      # }
    }
    config.ssh.forward_agent = true
  end
end
