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
        "python-dotenv",
        "neo4j-driver",
        "py2neo"
      ].join(" ")
    },
    :storage => [
      "mongodb-server",
      "mysql-server",
      "redis-server",
      "neo4j"
    ].join(" ")
  }

  config.vm.provision "shell", inline: <<-SHELL
    wget --no-check-certificate -O - https://debian.neo4j.org/neotechnology.gpg.key | apt-key add -
    echo 'deb http://debian.neo4j.org/repo stable/' | tee /etc/apt/sources.list.d/neo4j.list
    apt-get update
    locale-gen "en_US.UTF-8" "fi_FI.UTF-8"
    echo -e 'LANG="fi_FI.UTF-8"\nLANGUAGE="en_US:en"\nLC_ALL="fi_FI.UTF-8"\n' > /etc/default/locale
    apt-get install -y #{pysci_config[:basic_packages]}
    apt-get install -y #{pysci_config[:other]}
    apt-get install -y #{pysci_config[:python][:packages]}
    debconf-set-selections <<< "mysql-server mysql-server/root_password password #{pysci_config[:default_credentials][:password]}"
    debconf-set-selections <<< "mysql-server mysql-server/root_password_again password #{pysci_config[:default_credentials][:password]}"
    apt-get install -y #{pysci_config[:storage]}
    curl -i -H 'content-type: applciation/json' -d '{"password":"#{pysci_config[:default_credentials][:password]}"}' http://neo4j:neo4j@localhost:7474/user/neo4j/password
  SHELL

  config.vm.provision "shell", privileged:false, inline: <<-SHELL
    pip install -U #{pysci_config[:python][:pip_packages]}
  SHELL

end
