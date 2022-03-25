Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = "1024"
    v.cpus = 2
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "70"]
  end

  #######################
  ### MESOS COMPONENT ###
  #######################

  mesos_masters = [
    "10.0.1.31",
    "10.0.1.32",
    "10.0.1.33",
  ]

  mesos_masters.each_with_index do |ip, idx|
    id = idx+1
    config.vm.define "mesos_master_#{id}" do |mm_config|
      mm_config.vm.box = "bento/centos-6.10"
      mm_config.vm.host_name = "mesos-master-#{id}.test.dev"
      mm_config.vm.network :private_network, ip: ip

      mm_config.ssh.forward_agent = true

      mm_config.vm.provision "ansible" do |ansible|
        ansible.playbook = "deploy_mesos_master.yml"
        ansible.inventory_path = "vagrant_hosts"
        ansible.extra_vars = {:zookeeper_id => "#{id}"}
        ansible.tags = ansible_tags
        ansible.verbose = ansible_verbosity
        ansible.extra_vars = ansible_extra_vars
        ansible.limit = ansible_limit
      end
    end
  end

  # Mesos Agents
  mesos_agents = [
    "10.0.1.34",
    "10.0.1.35",
    "10.0.1.36",
  ]

  mesos_agents.each_with_index do |ip, idx|
    id = idx+1
    config.vm.define "mesos_agent_#{id}" do |mm_config|
      mm_config.vm.box = "bento/centos-6.10"
      mm_config.vm.host_name = "mesos-agent-#{id}.test.dev"
      mm_config.vm.network :private_network, ip: ip

      mm_config.ssh.forward_agent = true

      mm_config.vm.provision "ansible" do |ansible|
        ansible.playbook = "deploy_mesos_agent.yml"
        ansible.inventory_path = "vagrant_hosts"
        ansible.tags = ansible_tags
        ansible.verbose = ansible_verbosity
        ansible.extra_vars = ansible_extra_vars
        ansible.limit = ansible_limit
      end
    end
  end
end
