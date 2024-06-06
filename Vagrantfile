Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # Обновление списка пакетов
    sudo apt-get update
    
    # Установка зависимостей для сборки PostgreSQL
    sudo apt-get install -y build-essential libreadline-dev zlib1g-dev flex bison
    
    # Скачивание и распаковка исходников PostgreSQL 8.4
    wget https://ftp.postgresql.org/pub/source/v8.4.22/postgresql-8.4.22.tar.bz2
    tar -xjf postgresql-8.4.22.tar.bz2
    cd postgresql-8.4.22
    
    # Сборка и установка PostgreSQL 8.4
    ./configure
    make
    sudo make install
    
    # Создание пользователя и директории данных
    sudo useradd postgres
    sudo mkdir /usr/local/pgsql/data
    sudo chown postgres /usr/local/pgsql/data
    
    # Инициализация базы данных
    sudo -u postgres /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
    
    # Запуск PostgreSQL
    sudo -u postgres /usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile start
    
    # Установка пароля для пользователя postgres
    sudo -u postgres /usr/local/pgsql/bin/psql -c "ALTER USER postgres PASSWORD 'password';"
    
    # Добавление PostgreSQL в PATH
    echo 'export PATH=$PATH:/usr/local/pgsql/bin' >> ~/.bashrc
    source ~/.bashrc
  SHELL
end
