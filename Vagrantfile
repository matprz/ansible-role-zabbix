# By: mrlucasfreitas
# https://github.com/mrlucasfreitas

# Install vagrant-hostsupdater plugin
unless Vagrant.has_plugin? 'vagrant-hostsupdater'
  system('vagrant plugin install vagrant-hostsupdater')
end

# Install public key in VM
# The public key must be located in ~/.ssh/id_rsa
ssh_pub_key = File.read("#{Dir.home}/.ssh/id_rsa.pub")

ssh_setup_script = <<-SCRIPT
  ssh_authorized_keys="/home/vagrant/.ssh/authorized_keys"
  echo '#{ssh_pub_key}' >> "$ssh_authorized_keys"
SCRIPT

# Ansible variables
ANSIBLE_CONFIG = File.join(File.dirname(__FILE__), "ansible.cfg")
ansible_provision_playbook = ENV["ANSIBLE_PROVISION_PLAYBOOK"] || "test.yml"

# Define VM images and hostnames
hosts = [
  {
    :name     => "centos7",
    :box      => "bento/centos-7",
    :box_v    => "202002.04.0",
    :vm_name  => "vagrant-centos7",
    :hostname => "vagrant-centos7.local",
    :ip       => "172.16.10.53"
  },
  {
    :name     => "centos8",
    :box      => "bento/centos-8",
    :box_v    => "202012.21.0",
    :vm_name  => "vagrant-centos8",
    :hostname => "vagrant-centos8.local",
    :ip       => "172.16.10.55"
  },
  {
    :name     => "amazonlinux-2",
    :box      => "bento/amazonlinux-2",
    :box_v    => "1.2.1",
    :vm_name  => "vagrant-amazonlinux2",
    :hostname => "vagrant-amazonlinux2.local",
    :ip       => "172.16.10.59"
  }
]

# Define VM configuration
Vagrant.configure("2") do |config|
  hosts.each do |host|
    config.vm.define host[:name] do |m|
      m.vm.box = host[:box]
      m.vm.box_version = host[:box_v]
      m.vm.hostname = host[:hostname]
      m.vm.network :private_network, ip: host[:ip]

      if host[:name] =~ /^freebsd/ then
        m.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true
        m.ssh.shell = "sh"
        m.vm.base_mac = host[:base_mac]
        m.vm.boot_timeout = 600
      end

      m.vm.provider :virtualbox do |vb|
        vb.name = host[:vm_name]
        vb.gui = false
        vb.cpus = 2
        vb.memory = 1024
      end
      m.vm.provision "shell", inline: ssh_setup_script, privileged: false
    end
  end
end
