Vagrant.configure(2) do |config|
  arch = %w[32 64].include?(ENV['ARCH']) ? ENV['ARCH'] : '64'
  config.vm.box = "karolistamutis/archlinux#{arch}"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
  config.vm.provision "ansible" do |ansible|
    ansible.raw_arguments = "-e arch=#{arch}"
    ansible.extra_vars = { 
      ansible_python_interpreter:"/usr/bin/python2" 
    }
    
    ansible.playbook = "provisioning/playbook.yml"
  end
end

