# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
#  config.vm.provider "virtualbox" do |vb|
# Display the VirtualBox GUI when booting the machine
#     vb.gui = true
#     vb.memory = "1024"
#  end
  config.vm.synced_folder "./", "/vagrant"

  config.vm.define "mysql" do |sql|
    sql.vm.box = "ubuntu/xenial64"
    sql.vm.hostname = "mysql"
#    ps.ssh.username = "vagrant"
#    ps.ssh.password = "vagrant"
#    ps.vm.network "forwarded_port", guest: 5432, host: 5432
    sql.vm.network :private_network, ip: "192.168.0.101"
    sql.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.memory = "512"
    end
    sql.vm.provision :chef_solo do |chef|
         chef.cookbooks_path = "/home/danil/chef-repo/cookbooks"
         chef.roles_path = "/home/danil/chef-repo/roles"
         chef.add_role "mysql"
         chef.json.merge!({
             :mysql => {
                 :server_root_password => "root" # TODO Hardcoded MySQL root password.
             },
         })
    end
  end

  config.vm.define "apache" do |ap|
    ap.vm.box = "ubuntu/xenial64"
    ap.vm.hostname = "apache"
    ap.vm.network "forwarded_port", guest: 80, host: 8888
    ap .vm.network :private_network, ip: "192.168.0.102"
    ap.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.memory = "1024"
    end
    ap.vm.provision :chef_solo do |chef|
         chef.cookbooks_path = "/home/danil/chef-repo/cookbooks"
         chef.roles_path = "/home/danil/chef-repo/roles"
         chef.add_role "apache"
#        chef.add_recipe "apache2"
#        chef.json = { :apache => { :default_site_enabled => true } }
      end
   end
end
