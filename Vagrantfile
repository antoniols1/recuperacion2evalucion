$init = <<SCRIPT
  timedatectl set-timezone Europe/Madrid
  apt update
  apt install net-tools
SCRIPT
 
$docker = <<SCRIPT
  apt install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
 
  mkdir -p /etc/apt/keyrings
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
 
  echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
 
  apt update
 
  apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
SCRIPT
 
Vagrant.configure("2") do |config|
 
    config.vm.define :docker, primary: true do |docker|
 
      docker.vm.box = "ubuntu/focal64"
      docker.vm.box_check_update = true
 
      #docker.vm.disk :disk, size: "50GB", primary: true
 
      docker.vm.network "private_network", ip: "10.100.150.200"
 
      docker.vm.hostname = "ubuntu-docker"
 
      docker.vm.provider "virtualbox" do |v|
        v.name = "ubuntu-docker"
        v.memory = "2048"
        v.cpus = 2
      end
 
      docker.vm.provision "shell", inline: $init, privileged: true
      docker.vm.provision "shell", inline: $docker, privileged: true
 
    end
end