# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

ENV['MYSQL_ROOT_PASS'] = "super-secret"
ENV['WP_DB_PASS'] = "not-so-secret"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = 'ubuntu/trusty64'

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network 'forwarded_port', guest: 80, host: 8080

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :storyteller do |storyteller|
    storyteller.vm.hostname = 'storyteller'
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  config.vm.provider "virtualbox" do |v|
    host = RbConfig::CONFIG['host_os']

    # Give VM 1/4 system memory & access to all cpu cores on the host
    if host =~ /darwin/
      cpus = `sysctl -n hw.ncpu`.to_i
      # sysctl returns Bytes and we need to convert to MB
      mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
    elsif host =~ /linux/
      cpus = `nproc`.to_i
      # meminfo shows KB and we need to convert to MB
      mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
    else # Windows (shrugs)
      cpus = 2
      mem = 1024
    end

    v.customize ['modifyvm', :id, '--memory', mem]
    v.customize ['modifyvm', :id, '--cpus', cpus]
  end

  # Ansible provider
  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'test.yml'
    ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
    ansible.host_key_checking = false
  end
end
