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
        "python-sympy",
        "python-matplotlib",
        "python-pandas",
        "python-nltk",
        "python-statsmodels",
        "python-rpy2",
        "python-pymongo",
        "python-mysqldb",
        "python-mysql.connector",
        "python-redis",
        "python-couchdb",
        "python-pycassa",
        "python-pysqlite2",
        "python-wxgtk3.0"
      ].join(" "),
      :pip_packages => [
        "scikit-learn",
        "python-dotenv",
        "pybrain",
        "seaborn",
        "tensorflow",
        "neo4j-driver",
        "py2neo",
        "cassandra-driver==3.10",
        "click",
        "sqlalchemy"
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
    cat << EOF >> ~/.bash_aliases
alias new-python='echo -e "#!/usr/bin/env python\n# -*- coding: utf-8 -*-\n\nimport sys, os\nimport numpy as np\nimport matplotlib.pyplot as plt\nimport pandas as pd\nimport statsmodels.api as sm\n\ndef main(argc, argv, srcdir):\n    #put code here\n    return 0\n\n\nif __name__ == \"__main__\":\n    sys.exit(main(len(sys.argv), sys.argv, os.path.dirname(os.path.realpath(__file__))))"'
alias new-bash='echo -e "#!/usr/bin/env bash\n\n#put code here"'
EOF
  SHELL

end
