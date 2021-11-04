# -*- mode: ruby -*-
# vi: set ft=ruby :

vms = {
  'm1' => {'memory' => '512', 'cpus' => 1, 'ip' => '10'},
  'm2' => {'memory' => '512', 'cpus' => 1, 'ip' => '20'},
  'm3' => {'memory' => '512', 'cpus' => 1, 'ip' => '30'}
}

Vagrant.configure('2') do |config|

  config.vm.box = 'debian/buster64'
  config.vm.box_check_update = false

  vms.each do |name, conf|
    config.vm.define "#{name}" do |m|
      m.vm.hostname = "#{name}.ansible.local"
      m.vm.network 'private_network', ip: "172.27.11.#{conf['ip']}"

      m.vm.provider 'virtualbox' do |vb| # VirtualBox
        vb.memory = conf['memory']
        vb.cpus = conf['cpus']
      end
      m.vm.provider 'libvirt' do |lv| # Libvirt
        lv.memory = conf['memory']
        lv.cpus = conf['cpus']
        lv.cputopology :sockets => 1, :cores => conf['cpus'], :threads => '1'
      end

      m.vm.provision "shell", inline: <<-'SHELL'
        apt-get update
        apt-get install -y ansible vim nano
	mkdir -p /root/.ssh
        cp /vagrant/files/id_rsa* /root/.ssh
        chmod 400 /root/.ssh/id_rsa*
        cp /vagrant/files/id_rsa.pub /root/.ssh/authorized_keys
      SHELL

    end

  end

end
