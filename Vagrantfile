# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "pysci-box"
  config.ssh.forward_x11 = true

  pysci_config = {
    :basic_packages => [
      "build-essential",
      "git",
      "curl",
      "tree",
      "screen",
      "zip",
      "unzip"
    ].join(" "),
    :default_credentials => {
      :user => "pysci",
      :password => "pysci"
    },
    :other => [
      "r-base",
      "octave",
      "lua50",
      "scala",
      "clojure1.6"
    ].join(" "),
    :python => {
      :packages => [
        "python-pip",
        "python-scipy",
        "python-numpy",
        "python-matplotlib",
        "python-pandas",
        "python-pymongo",
        "python-mysqldb",
        "python-mysql.connector",
        "python-redis"
      ].join(" "),
      :pip_packages => [
        "scikit-learn",
        "python-dotenv"
      ].join(" ")
    },
    :storage => [
      "mongodb-server",
      "mysql-server",
      "redis-server"
    ].join(" ")
  }

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    locale-gen "en_US.UTF-8" "fi_FI.UTF-8"
    echo -e 'LANG="fi_FI.UTF-8"\nLANGUAGE="en_US:en"\nLC_ALL="fi_FI.UTF-8"\n' > /etc/default/locale
    apt-get install -y #{pysci_config[:basic_packages]}
    apt-get install -y #{pysci_config[:other]}
    apt-get install -y #{pysci_config[:python][:packages]}
    debconf-set-selections <<< "mysql-server mysql-server/root_password password #{pysci_config[:default_credentials][:password]}"
    debconf-set-selections <<< "mysql-server mysql-server/root_password_again password #{pysci_config[:default_credentials][:password]}"
    apt-get install -y #{pysci_config[:storage]}
  SHELL

  config.vm.provision "shell", privileged:false, inline: <<-SHELL
    pip install -U #{pysci_config[:python][:pip_packages]}
  SHELL

end
