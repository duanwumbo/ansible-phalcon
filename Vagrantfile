# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set tabstop=2 :
# vi: set shiftwidth=2 :

Vagrant.configure("2") do |config|

  vms_debian = [
    { :name => "debian-jessie-php56",  :box => "debian/jessie64",  :vars => { },                      :groups => ['regular'] },
    { :name => "debian-jessie-php70",  :box => "debian/jessie64",  :vars => { "php_version": '7.0' }, :groups => ['sury'] },
    { :name => "debian-jessie-php71",  :box => "debian/jessie64",  :vars => { "php_version": '7.1' }, :groups => ['sury'] },
    { :name => "debian-stretch-php70", :box => "debian/stretch64", :vars => { },                      :groups => ['regular'] },
    { :name => "debian-stretch-php71", :box => "debian/stretch64", :vars => { "php_version": '7.1' }, :groups => ['sury'] }
  ]

  conts = [
    { :name => "docker-debian-jessie-php56",  :docker => "hanxhx/vagrant-ansible:debian8", :vars => { },                      :groups => ['regular'] },
    { :name => "docker-debian-jessie-php70",  :docker => "hanxhx/vagrant-ansible:debian8", :vars => { "php_version": '7.0' }, :groups => ['sury'] },
    { :name => "docker-debian-jessie-php71",  :docker => "hanxhx/vagrant-ansible:debian8", :vars => { "php_version": '7.1' }, :groups => ['sury'] },
    { :name => "docker-debian-stretch-php70", :docker => "hanxhx/vagrant-ansible:debian9", :vars => { },                      :groups => ['regular'] },
    { :name => "docker-debian-stretch-php71", :docker => "hanxhx/vagrant-ansible:debian9", :vars => { "php_version": '7.1' }, :groups => ['sury'] }
  ]

  config.vm.network "private_network", type: "dhcp"

  conts.each do |opts|
    config.vm.define opts[:name] do |m|
      m.vm.provider "docker" do |d|
        d.image = opts[:docker]
        d.remains_running = true
        d.has_ssh = true
      end
      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.verbose = 'vv'
        ansible.sudo = true
        ansible.extra_vars = opts[:vars]
        ansible.groups = { opts[:groups][0] => [opts[:name]] }
      end
    end
  end

  vms_debian.each do |opts|
    config.vm.define opts[:name] do |m|
      m.vm.box = opts[:box]
      m.vm.provider "virtualbox" do |v|
        v.cpus = 1
        v.memory = 256
      end
      m.vm.provision "ansible" do |ansible|
        ansible.playbook = "tests/test.yml"
        ansible.verbose = 'vv'
        ansible.sudo = true
        ansible.extra_vars = opts[:vars]
        ansible.groups = { opts[:groups][0] => [opts[:name]] }
      end
    end
  end
end
