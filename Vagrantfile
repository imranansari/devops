Vagrant.configure(2) do |config|

  VAGRANT_VM_PROVIDER = "virtualbox"

#  config.vm.box_check_update = false
 
  local_user_home = File.expand_path('~') 
  local_public_key = File.read("#{local_user_home}/.ssh/id_rsa.pub")
  local_private_key = File.read("#{local_user_home}/.ssh/id_rsa")

  config.vm.synced_folder "./", "/vagrant", disabled:true
  config.vm.synced_folder "./", "/home/vagrant/sync", disabled:true
  config.ssh.insert_key = false

  config.vm.define "master" do |machine|
    machine.vm.box = "ubuntu/trusty64"
    machine.vm.network "private_network", ip:"192.168.56.210"
    machine.vm.hostname = "master"
    machine.vm.provider "virtualbox" do |vb|
      vb.name = "master-esgi"
      vb.cpus = 2
      vb.memory = 4*1024
    end
  end

  config.vm.define "slave" do |machine|
    machine.vm.box = "centos/7"
    machine.vm.network "private_network", ip:"192.168.56.211"
    machine.vm.hostname = "slave"
    machine.vm.provider "virtualbox" do |vb|
      vb.name = "slave-esgi"
      vb.cpus = 2
      vb.memory = 4*1024
    end
  end

  config.vm.provision :shell, :inline =>"
    echo 'Copying public SSH Keys to the VM'
    mkdir -p /home/vagrant/.ssh
    chmod 700 /home/vagrant/.ssh
    echo '#{local_public_key}' >> /home/vagrant/.ssh/authorized_keys
    chmod -R 600 /home/vagrant/.ssh/authorized_keys
    echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
    echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
    echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
    chmod -R 600 /home/vagrant/.ssh/config
  ", privileged: false

#  config.vm.provision "file", source: "#{local_private_key}", destination: "/home/vagrant/.ssh/id_rsa"

end


       
