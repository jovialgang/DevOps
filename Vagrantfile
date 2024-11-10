#ENV["LC_ALL"] = "en_US.UTF-8"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  # Кол-во машин
  SERVERS = 5
  # Им] сетевого интерфейса
  BRIDGE = "vboxnet0"

  def create_host(config, hostname, ip)
    config.vm.define hostname do |host|
      host.vm.network "private_network", ip: ip
      host.vm.network "public_network", bridge: BRIDGE
      host.vm.hostname = hostname
      host.vm.provider "virtualbox" do |vb|
        vb.memory = 1024  # 1GB of RAM
        vb.cpus = 1      # 1 CPU
        #vb.gui = true
      end

      # Установка пакета питона
      host.vm.provision "shell", inline: <<-SHELL
       apt-get update
       sudo apt-get install python3-pip -y
      SHELL

      yield host if block_given?
    end
  end

  # Создание машин (с srv1 по srv5)
  (1..SERVERS).each do |machine_id|
    create_host(config, "srv#{machine_id}", "192.168.56.#{200 + machine_id}")
  end
end


