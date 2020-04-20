# -*- mode: ruby -*-
require 'shellwords'

Vagrant.configure('2') do |config|
  withES = true

  config.vm.box = 'debian/buster64'

  config.vm.hostname = 'prestashop'

  config.vm.network 'public_network'
  config.vm.network 'private_network', ip: '192.168.42.42'
  config.vm.provision :file, source: './files/phpmyadmin.conf', destination: '/tmp/phpmyadmin.conf'
  config.vm.provision :file, source: './files/000-default.conf', destination: '/tmp/000-default.conf'
  config.vm.provision :file, source: './files/gitconfig', destination: '.gitconfig'
  config.vm.provision :file, source: './files/gitconfig', destination: '/tmp/gitconfig'
  args = [
    Shellwords.escape(ENV['PHP_VERSION']),
    Shellwords.escape(ENV['PR']),
    Shellwords.escape(ENV['BRANCH']),
    Shellwords.escape(ENV['AUTOMATIC_INSTALL']),
    Shellwords.escape(ENV['MODULE'])
  ]
  config.vm.provision :shell, path: 'provisioning/script.sh', args: args.join(' ')

  if withES then
    config.vm.synced_folder "../ESearch/src", "/var/www/prestashop/modules/cs_esearch"
    config.vm.provision :shell, name: 'elasticsearch', path: 'provisioning/elasticsearch.sh'
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  config.vm.provider 'virtualbox' do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = '2048'
  end
end
