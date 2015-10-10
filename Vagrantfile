# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.omnibus.chef_version = :latest

  # Enable caching if plugin installed
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.box = "trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.vm.network :forwarded_port, guest: 3306, host: 33066
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :private_network, ip: "192.168.33.33"

  host = RbConfig::CONFIG['host_os']

  # Use nfs when running on a unix system
  if host =~ /darwin/
	  config.vm.synced_folder ".", "/vagrant", nfs: true
  elsif host =~ /linux/
	  config.vm.synced_folder ".", "/vagrant", nfs: true
  else
	  config.vm.synced_folder ".", "/vagrant", mount_options: ['dmode=777','fmode=666']
  end

  # Assign reasonable memory and cpu cores
  config.vm.provider :virtualbox do |v|
      # Give VM 1/4 system memory & access to all cpu cores on the host
      if host =~ /darwin/
        cpus = `sysctl -n hw.ncpu`.to_i
        # sysctl returns Bytes and we need to convert to MB
        mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
      elsif host =~ /linux/
        cpus = `nproc`.to_i
        # meminfo shows KB and we need to convert to MB
        mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
      else
        cpus = 2
        mem = 4096
      end

      v.customize ["modifyvm", :id, "--memory", mem]
      v.customize ["modifyvm", :id, "--cpus", cpus]
      v.gui = true
  end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks","site-cookbooks"]
    chef.json = {
        'ts_basebox' => {
            'elasticsearch' => {
                'install' => true
            },
            'solr' => {
                'install' => true
            }
        },
        'ts_phpapp' => {
        	'apps' => [
				{
					'name' => 'superkicker',
					'shortname' => 'sk',
					'hostname' => 'superkicker.devbox.local',
					'source' => {
						'type' => 'git',
						'repository' => 'https://github.com/timoschmidt/superkicker.git',
						'branch' => 'master'
					},
					'deployment' => [
						'composer install',
						'php app/console doctrine:database:create',
						'php app/console doctrine:schema:update --force'
					]
				}
			]
        }
    }
    chef.add_recipe "ts_phpapp"

  end
end
