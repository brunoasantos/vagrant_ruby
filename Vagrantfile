VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', '2048']
  end

  config.vm.synced_folder '~/work', '/home/vagrant/code'

  config.vm.network :forwarded_port, guest: 3000, host: 3000

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ['cookbooks']

    chef.add_recipe 'apt'
    chef.add_recipe 'mysql::server'
    chef.add_recipe 'ohmyzsh'
    chef.add_recipe 'system_packages'
    chef.add_recipe 'nodejs'

    chef.json = {
      mysql: {
        server_root_password: '',
        server_repl_password: '',
        server_debian_password: ''
      },
      rvm: {
        user_installs: [{
          user: 'vagrant',
          rubies: ['2.1.2', '1.9.3', '1.8.7'],
          global: '2.1.2',
          gems: {
            '2.1.2' => [
              { name: 'bundler' }
            ]
          }
        }]
      },
      system_packages: {
        packages: 'imagemagick libmagickwand-dev tmux vim libmysql-ruby'
      }
    }

    chef.log_level = :debug
  end
end
