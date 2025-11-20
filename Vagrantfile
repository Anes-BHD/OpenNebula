Vagrant.configure("2") do |config|

config.vm.define "frontend" do |frontend|
frontend.vm.box = "OpenNebula/frontend"  # Published box
frontend.vm.hostname = "frontend"


frontend.vm.provider "virtualbox" do |vb|
  vb.gui = true
  vb.memory = "2048"
  vb.cpus = 2
end

# eth0: NAT (internet)
# eth1: Management network
frontend.vm.network "private_network", ip: "192.168.56.10"

# eth2: OpenNebula VM network (bridged)
frontend.vm.network "private_network", ip: "192.168.56.20", auto_config: false

# Provisioning script
frontend.vm.provision "shell", inline: <<-SHELL
  sudo ip link add name br0 type bridge 2>/dev/null || true
  sudo ip addr flush dev eth2
  sudo ip link set dev eth2 master br0
  sudo ip link set dev eth2 up
  sudo ip link set dev br0 up
  sudo ip addr add 192.168.56.20/24 dev br0
  echo "Bridge br0 created and ready for OpenNebula"
SHELL


end

config.vm.define "node1" do |node1|
node1.vm.box = "OpenNebula/node"  # Published box
node1.vm.hostname = "node1"


node1.vm.provider "virtualbox" do |vb|
  vb.memory = "1024"
  vb.cpus = 1
end

# eth0: NAT (internet)
# eth1: Management network
node1.vm.network "private_network", ip: "192.168.56.11"

# eth2: OpenNebula VM network (bridged)
node1.vm.network "private_network", ip: "192.168.56.21", auto_config: false

# Provisioning script
node1.vm.provision "shell", inline: <<-SHELL
  sudo ip link add name br0 type bridge 2>/dev/null || true
  sudo ip addr flush dev eth2
  sudo ip link set dev eth2 master br0
  sudo ip link set dev eth2 up
  sudo ip link set dev br0 up
  sudo ip addr add 192.168.56.21/24 dev br0
  echo "Bridge br0 created and ready for OpenNebula"
SHELL


end

end
