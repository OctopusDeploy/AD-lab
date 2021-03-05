Vagrant.configure("2") do |config|
  
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
  end

  config.vm.define "dc" do |dc|
    dc.vm.guest = :windows
    dc.vm.communicator = "winrm"
    dc.vm.boot_timeout = 600
    dc.vm.graceful_halt_timeout = 600
    dc.winrm.retry_limit = 30
    dc.winrm.retry_delay = 10
    dc.vm.box = "StefanScherer/windows_2019"
    dc.vm.network "private_network", ip: "192.168.56.10"
    dc.vm.network :forwarded_port, guest: 3389, host: 23389, id: "msrdp"
    dc.vm.network :forwarded_port, guest: 5985, host: 25985, id: "winrm"
    dc.vm.provision "shell", path:"ConfigureRemotingForAnsible.ps1"
  end

  config.vm.define "win_server" do |win_server|
    win_server.vm.guest = :windows
    win_server.vm.communicator = "winrm"
    win_server.vm.boot_timeout = 600
    win_server.vm.graceful_halt_timeout = 600
    win_server.winrm.retry_limit = 30
    win_server.winrm.retry_delay = 10
    win_server.vm.box = "StefanScherer/windows_2019"
    win_server.vm.network "private_network", ip: "192.168.56.11"
    win_server.vm.network :forwarded_port, guest: 3389, host: 33389, id: "msrdp"
    win_server.vm.network :forwarded_port, guest: 5985, host: 35985, id: "winrm"
    win_server.vm.network :forwarded_port, guest: 8080, host: 8080, id: "octopus"
    win_server.vm.provision "shell", path:"ConfigureRemotingForAnsible.ps1"
  end

end
