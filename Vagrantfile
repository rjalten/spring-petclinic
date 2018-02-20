cluster = {
  "node1"   => { :ip => "192.168.100.11", :cpus => 2, :mem => 2048 },
  "node2"   => { :ip => "192.168.100.12", :cpus => 2, :mem => 2048 },
  "jenkins" => { :ip => "192.168.100.20", :cpus => 1, :mem => 1024 },
}

Vagrant.configure("2") do |config|
  cluster.each_with_index do |(hostname, info), index|
    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
        config.vm.box = "centos/7"
        override.vm.hostname = hostname
        vb.name = hostname
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus]]
      end
    end
  end
end
