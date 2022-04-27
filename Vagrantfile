N = 2

Vagrant.configure("2") do |config|
  config.vm.provider :docker do |d|
     d.build_dir = "."
     d.remains_running = true
     d.has_ssh = true
  end
  
  config.vm.define "k8s-master" do |master|
    master.vm.network "private_network", ip: "192.168.50.10"  
    master.vm.hostname = "k8s-master"
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "kubernetes-setup/master-playbook.yml"
      ansible.extra_vars = {
          node_ip: "192.168.50.10",
      }
    end 
  end

  (1..N).each do |i|
    config.vm.define "ks8node-#{i}" do |node|
        node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
        node.vm.hostname = "ks8node-#{i}"  
        node.vm.provision "ansible" do |ansible|
          ansible.playbook = "kubernetes-setup/node-playbook.yml"
          ansible.extra_vars = {
              node_ip: "192.168.50.#{i + 10}",
          } 
        end      
    end 
  end 
end  