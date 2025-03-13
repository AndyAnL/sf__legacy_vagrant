Vagrant.configure("2") do |config|
  # Используем образ Ubuntu 18.04
  config.vm.box = "ubuntu/bionic64"

  # Настройка сети (не обязательно но пусть будет)
  config.vm.network "private_network", ip: "192.168.33.10"

  # Установка PostgreSQL 8.4 из исходников
  config.vm.provision "shell", inline: <<-SHELL
    # Обновляем пакеты
    apt-get update

    # Устанавливаем зависимости для сборки PostgreSQL
    apt-get install -y build-essential libreadline-dev zlib1g-dev wget

    # Скачиваем исходники PostgreSQL 8.4
    wget https://ftp.postgresql.org/pub/source/v8.4.22/postgresql-8.4.22.tar.gz
    tar -xvzf postgresql-8.4.22.tar.gz
    cd postgresql-8.4.22

    # Собираем и устанавливаем PostgreSQL
    ./configure
    make
    make install

    # Создаем пользователя и базу данных
    adduser --disabled-password --gecos "" postgres
    mkdir /usr/local/pgsql/data
    chown postgres /usr/local/pgsql/data
    sudo -u postgres /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data

    # Запускаем PostgreSQL
    sudo -u postgres /usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile start

    # Устанавливаем переменные окружения
    echo 'export PATH=/usr/local/pgsql/bin:$PATH' >> /home/vagrant/.bashrc
    echo 'export PGDATA=/usr/local/pgsql/data' >> /home/vagrant/.bashrc
  SHELL
end