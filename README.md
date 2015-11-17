```
# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
# APT-GET
sudo apt-get update
sudo apt-get upgrade

# GIT
sudo apt-get install git

# curl
sudo apt-get -y install curl
sudo apt-get install ca-certificates

# Node JS
sudo apt-get -y --purge remove node
sudo apt-get -y --purge remove nodejs-legacy
sudo apt-get -y --purge remove nodejs
sudo apt-get install -y nodejs-legacy

# build essentials?
sudo apt-get install -y build-essential libssl-dev libffi-dev

$ other curl stuff; helps with nvm below
sudo mkdir -p /etc/pki/tls/certs
sudo cp /etc/ssl/certs/ca-certificates.crt /etc/pki/tls/certs/ca-bundle.crt

# PIP
sudo apt-get -y install python-pip
sudo pip install --upgrade pip
sudo pip install --upgrade virtualenv

# ANACONDA
anaconda=Anaconda-2.3.0-Linux-x86_64.sh
if [[ ! -f $anaconda ]]; then
	wget --quiet https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/$anaconda
fi
chmod +x $anaconda
./$anaconda -b -p /home/vagrant/anaconda
cat >> /home/vagrant/.bashrc << END
export PATH=/home/vagrant/anaconda/bin:$PATH
END
rm $anaconda

# NVM
curl https://raw.githubusercontent.com/creationix/nvm/v0.16.1/install.sh | sh
source ~/.profile

# NPM
sudo apt-get install -y npm

npm install express-generator -g
npm install -g nodemon
npm install -g forever

SCRIPT


Vagrant.configure(2) do |config|

  config.vm.define "dev"
  
  config.vm.box = "ubuntu/trusty64"
	
  config.vm.network "private_network", type: "dhcp"

  config.vm.provider :virtualbox do |v|
  	v.memory = 1024
  	v.cpus = 2
  end
  
  config.vm.provision :shell, inline: $script
  config.vm.synced_folder ".", "/home/projects"

end
```
