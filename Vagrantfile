Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu-18.04-amd64"

  config.vm.hostname = "gitlab.example.com"

  config.vm.network "private_network", ip: "10.10.9.99", libvirt__forward_mode: "route", libvirt__dhcp_enabled: false

  config.vm.provider 'libvirt' do |lv, config|
    lv.memory = 2048
    lv.cpus = 2
    lv.cpu_mode = 'host-passthrough'
    lv.keymap = 'pt'
    config.vm.synced_folder '.', '/vagrant', type: 'nfs'
  end

  config.vm.provider "virtualbox" do |vb|
    vb.linked_clone = true
    vb.memory = 2048
    vb.cpus = 2
  end

  config.trigger.before :up do |trigger|
    ldap_ca_cert_path = '../windows-domain-controller-vagrant/tmp/ExampleEnterpriseRootCA.der'
    trigger.run = {inline: "sh -c 'mkdir -p tmp && cp #{ldap_ca_cert_path} tmp'"} if File.file? ldap_ca_cert_path
  end

  config.vm.provision "shell", path: "provision-mailhog.sh"
  config.vm.provision "shell", path: "provision.sh"
  config.vm.provision "shell", path: "provision-gitlab-source-link-proxy.sh"
end
