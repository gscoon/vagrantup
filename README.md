# -*- mode: ruby -*-
# vi: set ft=ruby :

ipythonPort = 8888
ipythonHost = 4545
sparkUIPort = 4040
pulsePort 	= 1111

$script = <<SCRIPT
# APT-GET
sudo apt-get update
sudo apt-get upgrade



# GIT
sudo apt-get install git

# Node JS
sudo apt-get --purge remove node
sudo apt-get --purge remove nodejs-legacy
sudo apt-get --purge remove nodejs
sudo apt-get install nodejs-legacy

# PIP
apt-get -y install python-pip
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

# NPM
sudo apt-get install -y npm

npm install express-generator -g
npm install -g nodemon
npm install -g forever

SCRIPT


Vagrant.configure(2) do |config|

  config.vm.define "dev"
  
  config.vm.box = "hashicorp/precise64"
	
  config.vm.network "private_network", ip: "192.168.1.234"

  config.vm.provider :virtualbox do |v|
  	v.memory = 1024
  	v.cpus = 2
  end
  
  config.vm.provision :shell, inline: $script
  config.vm.synced_folder ".", "/home/projects"

end
