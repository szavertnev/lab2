Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos8s"
  config.vm.box_check_update = false

  # Настройки VM
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
    vb.name = "centos8s-kernel-upgrade"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # Настраиваем DNS
    echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf

    # Репозиторий из методички устарел, чиним
    sudo sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*.repo
    sudo sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*.repo

    # Обновляем систему
    sudo dnf clean all
    sudo dnf update -y

    # Устанавливаем ELRepo
    sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
    sudo dnf install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm

    # Устанавливаем ядро из ELRepo
    sudo dnf --enablerepo=elrepo-kernel install -y kernel-ml

    # Настраиваем GRUB
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    sudo grub2-set-default 0


  SHELL

  # Перезагрузку необходимо выполнить вручную
end
