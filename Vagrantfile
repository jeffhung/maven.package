# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos-6.5"
# config.vm.box = "ubuntu/trusty64"

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.provision "runner", type: "ansible" do |ansible|
    ansible.playbook = "site.yml"
  end

  config.vm.provision "builder", type: "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.extra_vars = { "maven_environment" => "build" }
  end

# config.vm.network :forwarded_port, host: 5984, guest: 5984  # couchdb
end
