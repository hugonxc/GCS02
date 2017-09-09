# Vagrantfile API/syntax version.
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Configuração para DB
  config.vm.define "db" do |db|
    #Especificação da box
    db.vm.box = "ubuntu/trusty64"
      
    #Encaminhamento da porta  
    db.vm.network "forwarded_port", guest: 5432, host: 5432
    
    #Identificação o ip  
    db.vm.network "private_network", ip: "192.168.1.11"
    
    #Configuração do provider
    db.vm.provider "virtualbox" do |vb|
      vb.name = "dj-database"
      vb.gui = false
      vb.memory = "512"
      
    #Comandos de configuração do ambiente
    db.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y postgresql
      
      sudo su - postgres -c "psql -f /vagrant/db-config.sql"
      sudo su - postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
      sudo su - postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
      sudo service postgresql restart
    SHELL
    end
  end

  
  # Configuração para Web
  config.vm.define "web" do |web|
    #Especificação da box
    web.vm.box = "ubuntu/trusty64"
      
    #Encaminhamento da porta  
    web.vm.network "forwarded_port", guest: 8000, host: 8000
    
    #Identificação o ip  
    web.vm.network "private_network", ip: "192.168.1.10"
    
    #Configuração do provider
    web.vm.provider "virtualbox" do |vb|
      vb.name = "dj"
      vb.gui = false
      vb.memory = "512"
      
    #Comandos de configuração do ambiente
    web.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
      sudo pip install django flake8 psycopg2
    SHELL
    end
  end
end