servers=[
    {
      :hostname => "db01",
      :box => "centos/8",
      :ip => "192.168.50.101",
      :role => "master" # Assuming db01 as master for demonstration
    },
    {
      :hostname => "web01",
      :box => "centos/8",
      :ip => "192.168.50.102",
      :role => "worker"
    },
    {
      :hostname => "web02",
      :box => "centos/8",
      :ip => "192.168.50.103",
      :role => "worker"
    },
    {
      :hostname => "proxy",
      :box => "centos/8",
      :ip => "192.168.50.104",
      :role => "worker"
    }
  ]

  # Path to the scripts
  init_script = "~/lab/bootstrap.sh"
  master_script = "~/labs/bootstrap_master.sh"
  worker_script = "~/lbas/bootstrap_worker.sh"

  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
      node.vm.network :private_network, ip: machine[:ip]

      # Provision all nodes with the initial setup script
      node.vm.provision "shell", path: init_script

      # Additional provisioning based on role
      if machine[:role] == "master"
        # Provision master node
        node.vm.provision "shell", path: master_script
      elsif machine[:role] == "worker"
        # Provision worker nodes
        node.vm.provision "shell", path: worker_script
      end

      node.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 512]
        v.customize ["modifyvm", :id, "--name", machine[:hostname]]
      end
    end
  end
end


