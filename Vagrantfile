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
      
      cfg.vm.privision "shell", inline: "sudo yum install -y git java-1.8.0-openjdk-devel vim"
      
      if hostname == 'jenkins'
        cfg.vm.provision "shell", inline: "sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo"
        cfg.vm.provision "shell", inline: "sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key"
        cfg.vm.provision "shell", inline: "sudo yum -y install jenkins"
      end

      if ['node1', 'node2'].include?(hostname)
        cfg.vm.provision "file", source: "petclinic.service", destination: "/etc/systemd/system/petclinic.service"
        cfg.vm.provision "shell", inline: "sudo systemctl daemon-reload"
        cfg.vm.provision "shell", inline: "sudo systemctl enable petclinic"
      end
      
    end
  end
end
