# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "pysci-box"
  config.ssh.forward_x11 = true

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    locale-gen "en_US.UTF-8" "fi_FI.UTF-8"
    echo -e 'LANG="fi_FI.UTF-8"\nLANGUAGE="en_US:en"\nLC_ALL="fi_FI.UTF-8"\n' > /etc/default/locale
    apt-get install -y \
      python-pip \
      python-scipy \
      python-numpy \
      python-matplotlib \
      python-pandas \
      python-pymongo \
      mongodb-server
  SHELL

  config.vm.provision "shell", privileged:false, inline: <<-SHELL
    pip install -U \
      scikit-learn \
      python-dotenv
  SHELL

end
