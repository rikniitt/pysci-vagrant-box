# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "pysci-box"
  config.ssh.forward_x11 = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 4
  end

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
        "python-nltk",
        "python-statsmodels",
        "python-seaborn",
        "python-pymongo",
        "python-mysqldb",
        "python-mysql.connector",
        "python-redis",
        "python-couchdb",
        "python-pycassa",
        "python-pysqlite2"
      ].join(" "),
      :pip_packages => [
        "scikit-learn",
        "python-dotenv",
        "pybrain",
        "tensorflow",
        "neo4j-driver",
        "py2neo",
        "cassandra-driver==3.10"
      ].join(" ")
    },
    :storage => [
      "mongodb-server",
      "mysql-server",
      "redis-server",
      "neo4j",
      "couchdb",
      "cassandra",
      "sqlite"
    ].join(" ")
  }

  config.vm.provision "shell", inline: <<-SHELL
    wget --no-check-certificate -O - https://debian.neo4j.org/neotechnology.gpg.key | apt-key add -
    wget --no-check-certificate -O - https://www.apache.org/dist/cassandra/KEYS | apt-key add -
    apt-key adv --keyserver pool.sks-keyservers.net --recv-key A278B781FE4B2BDA
    echo 'deb http://debian.neo4j.org/repo stable/' | tee /etc/apt/sources.list.d/neo4j.list
    echo 'deb http://www.apache.org/dist/cassandra/debian 311x main' | tee /etc/apt/sources.list.d/cassandra.sources.list
    apt-get update
    locale-gen "en_US.UTF-8" "fi_FI.UTF-8"
    echo -e 'LANG="fi_FI.UTF-8"\nLANGUAGE="en_US:en"\nLC_ALL="fi_FI.UTF-8"\n' > /etc/default/locale
    debconf-set-selections <<< "mysql-server mysql-server/root_password password #{pysci_config[:default_credentials][:password]}"
    debconf-set-selections <<< "mysql-server mysql-server/root_password_again password #{pysci_config[:default_credentials][:password]}"
    apt-get install -y #{pysci_config[:basic_packages]} #{pysci_config[:other]} #{pysci_config[:python][:packages]} #{pysci_config[:storage]}
  SHELL

  config.vm.provision "shell", privileged:false, inline: <<-SHELL
    pip install --no-cache-dir -U #{pysci_config[:python][:pip_packages]}
    python -m nltk.downloader all
  SHELL

end
