machines = {
  "k8s-master"         => {"memory" => "3048", "cpu" => "2", "ip" => "10", "image" => "bento/ubuntu-20.04"},
  "k8s-node-1"         => {"memory" => "2048", "cpu" => "1", "ip" => "11", "image" => "bento/ubuntu-20.04"},
  "k8s-node-2"         => {"memory" => "2048", "cpu" => "1", "ip" => "12", "image" => "bento/ubuntu-20.04"}
}

Vagrant.configure("2") do |config|
  config.vm.synced_folder "manifests/", "/k8s/manifests"
  config.vm.box_check_update = false
  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.network "private_network", ip: "192.168.56.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
      end
      machine.vm.provision "ansible_local" do |ansible|
        ansible.playbook = 'playbook.yml'
        ansible.become = true
        ansible.verbose = true
        ansible.extra_vars = {
          node_ip: "192.168.56.#{conf["ip"]}",
          apiserver_advertise_address: "192.168.56.#{conf["ip"]}"
        }
      end
    end
  end
end
