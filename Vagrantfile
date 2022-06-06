$script = <<-SCRIPT

# Install Ansible inside corcho
apt-get update
apt-get install -y python3-pip sshpass
su - vagrant -c 'pip3 install --user ansible jmespath'
su - vagrant -c 'ansible-galaxy collection install community.docker'

# Allow connections from ansible to corcho
sed -i 's/PasswordAuthentication\ no/PasswordAuthentication\ yes/g' /etc/ssh/sshd_config
systemctl restart sshd

# Install paasnoid
su - vagrant -c 'ansible-playbook -i /vagrant/hosts /vagrant/install.yml --limit corcho'

# Create VPN config for clients
su - vagrant -c 'ansible-playbook -i /vagrant/hosts /vagrant/build_openvpn_client.yml --limit corcho'

# You'll need to configure Emby to use /docker-share

SCRIPT


Vagrant.configure("2") do |config|
  config.vm.define "corcho" do |config|
    config.vm.box = "debian/bullseye64"
    config.vm.hostname = "corcho"

    # lan interface connected to clients
    config.vm.network "private_network", ip: "192.168.56.1"
    # access to the vpn server
    config.vm.network "forwarded_port", guest: 3342, host: 3342, protocol: "udp"
    # open this so transmission can see its port as open
    config.vm.network "forwarded_port", guest: 51413, host: 51413, protocol: "tcp"
    config.vm.network "forwarded_port", guest: 51413, host: 51413, protocol: "udp"

    config.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end

    config.vm.provision "shell", inline: $script
  end

end
