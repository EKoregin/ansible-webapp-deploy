# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian-bookworm64"
#   config.vm.box_version = "12.20240905.1"

  USERNAME = "vagrantuser"
  PASSWORD = "vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 3072 # Увеличиваем до 2 ГБ для Prometheus/Grafana
    vb.cpus = 2
  end

  # VM1: Веб-сервер 1 + PostgreSQL
  config.vm.define "web1" do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "Web1"
    end
    node.vm.network "private_network", ip: "192.168.56.11", virtualbox__intnet: "private-net"
    node.vm.network "public_network", ip: "192.168.1.51", bridge: "Intel(R) Ethernet Controller I226-V"
    node.vm.hostname = "web1"
  end

  # VM2: Веб-сервер 2
  config.vm.define "web2" do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "Web2"
    end
    node.vm.network "private_network", ip: "192.168.56.12", virtualbox__intnet: "private-net"
    node.vm.network "public_network", ip: "192.168.1.52", bridge: "Intel(R) Ethernet Controller I226-V"
    node.vm.hostname = "web2"
  end

  # VM3: Балансировщик + Мониторинг
  config.vm.define "lb" do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "LoadBalancer"
    end
    node.vm.network "private_network", ip: "192.168.56.13", virtualbox__intnet: "private-net"
    node.vm.network "public_network", ip: "192.168.1.53", bridge: "Intel(R) Ethernet Controller I226-V"
    node.vm.hostname = "lb"
  end

  # Установка Ansible и зависимостей
  config.vm.provision "shell", inline: <<-SHELL
    # Добавляем пользователя #{USERNAME}
    id #{USERNAME} &>/dev/null || useradd -m -s /bin/bash #{USERNAME}
    echo "#{USERNAME}:#{PASSWORD}" | chpasswd
    usermod -aG sudo #{USERNAME}

    # Разрешим вход по SSH
    mkdir -p /home/#{USERNAME}/.ssh
    cp /home/vagrant/.ssh/authorized_keys /home/#{USERNAME}/.ssh/
    chown -R #{USERNAME}:#{USERNAME} /home/#{USERNAME}/.ssh
    chmod 700 /home/#{USERNAME}/.ssh
    chmod 600 /home/#{USERNAME}/.ssh/authorized_keys

#     sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
#     systemctl restart ssh
    apt-get update
    apt-get install -y net-tools # Для ifconfig
  SHELL
end