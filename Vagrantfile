require 'vagrant-aws'
require 'yaml'

#Vagrant.configure('2') do |config|
#  config.vm.define "vagrantAWS-Stata16" do |stata16|
#    stata16.vm.box = 'aws-dummy'
#    stata16.vm.synced_folder ".", "/vagrant", disabled: true
#    stata16.vm.provider 'aws' do |aws,override|
#      aws_config = YAML::load_file(File.join(Dir.home, ".aws_secrets"))
#      aws.access_key_id = aws_config.fetch("access_key_id")
#      aws.secret_access_key = aws_config.fetch("secret_access_key")
#      aws.keypair_name = aws_config.fetch("keypair_name")
##       aws.instance_type = "t2.medium"
#      aws.instance_type = "t2.small"
##       aws.instance_type = "t2.micro"
#      aws.region = aws_config.fetch("aws_region")
#      aws.ami = aws_config.fetch("aws_ami")
#      aws.security_groups = aws_config.fetch("security_groups")
##       aws.security_groups = 'vagrant'
##       aws.security_groups = ['default']
#      aws.subnet_id = aws_config.fetch("subnet_id")
##       aws.tags = {
##         'Name'=> "vagrantAWS-EOD"
##       }
#
#      aws.tags = {
#        'Name'=> "vagrantAWS-Stata16"
#      }
#      aws.block_device_mapping = [{
#        'DeviceName' => '/dev/sda1',
#        'Ebs.VolumeSize' => 100,
#        'Ebs.DeleteOnTermination' => true,
##         'Ebs.DeleteOnTermination' => false,
#        'Ebs.VolumeType' => 'gp2'
##         'Ebs.VolumeType' => 'io1'
##         'Ebs.Iops' => 1000
##         'DeviceName' => AWS_DEVICE_NAME,
##         'Ebs.ValumeSize' => AWS_DEVICE_SIZE,
##         'Ebs.DeleteOnTermination' => false,
##         'Ebs.VolumeType' => AWS_DEVICE_VOL_TYPE,
#      }]
#
#      override.ssh.username = aws_config.fetch("ssh_username")
#      override.ssh.private_key_path = aws_config.fetch("private_key_path")
#    end
#    config.vm.provision "ansible" do |ansible|
##     ansible.playbook = "deploy_ancillaryRH7.yml"
##     ansible.playbook = "deploy_eodrole.yml"
#      ansible.playbook = "deploy_mesosRH7.yml"
##     ansible.groups = {
##       "vagrantAWS" => ["vagrantAWS-Data"]
##     }
#      ansible.inventory_path = ".vagrant_hosts"
#      ansible.verbose = "true"
#    end
#  end
#end

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = "2048"
    v.cpus = 2
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "70"]
#   v.default_nic_type = "82543GC"
  end
# config.trigger.after :up do |trigger|
#   run "subscription-manager register --username <username> --password <password> --auto-attach
#   trigger.name = "After-Up Trigger ..."
#   trigger.info = "Trigger Execution ..."
#   trigger.run = { path:"subscription-manager register --username <username> --password <password> --auto-attach"}
# end
  config.vm.define "mesosRH7" do |chromeRH7|
    mesosRH7.vm.box = "clouddood/RH7.5_baserepo"
    mesosRH7.vm.hostname = "emacsRH7"
    mesosRH7.vm.network "private_network", ip: "192.168.60.167"
#   mesosRH7.vm.network "private_network", ip: "192.168.60.167", nic_type: "virtio"
    mesosRH7.vm.provision "shell", :inline => "sudo echo '192.168.60.167 mesosRH7.local mesosRH7' >> /etc/hosts"

##  Use Main / Update in Vagrant provision command ### $vagrant provision --provision-with shell/main/update

    # Default
    mesosRH7.vm.provision "main", type: "ansible" do |ansible|
      ansible.playbook = "deploy_mesosRH7_DEV.local.yml"
#     ansible.playbook = "deploy_mesosRH7_DEV.local.yml"
#     ansible.playbook = "deploy_chromeTestRH7.yml"
      ansible.inventory_path = "vagrant_hosts"
      #ansible.tags = ansible_tags
      #ansible.verbose = ansible_verbosity
      #ansible.extra_vars = ansible_extra_vars
      #ansible.limit = ansible_limit
    end
    # Update
#   mesosRH7.vm.provision "update", type: "ansible" do |ansible|
#     ansible.playbook = "deploy_emacsPatchRH7_DEV.local.yml"
#     ansible.playbook = "deploy_emacsTestRH7.yml"
#     ansible.inventory_path = "vagrant_hosts"
#     #ansible.tags = ansible_tags
#     #ansible.verbose = ansible_verbosity
#     #ansible.extra_vars = ansible_extra_vars
#     #ansible.limit = ansible_limit
#   end
  end
# config.trigger.before :destroy do |trigger|
#   run "rm -Rf /tmp/abc/*"
    # subscription-manager remove --all
    # subscription-manager unregister
    # subscription-manager clean
#   trigger.name = "Destroy Trigger ..."
#   trigger.info = "Destroy Trigger Execution ..."
#   trigger.run = { path: "subscription-manager remove --all"}
#   trigger.run = { path: "subscription-manager unregister"}
#   trigger.run = { path: "subscription-manager clean"}
# end
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
