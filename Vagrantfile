Vagrant.configure("2") do |config|
	config.vm.box = "generic/fedora33"
	config.vm.box_check_update = false
	config.vm.provider "virtualbox" do |vb|
		vb.name = "torServerSite"
		vb.memory = "2048"
	end

	config.vm.define "tor" do |tor| 
		tor.vm.network "private_network", ip: "192.168.56.10"
		tor.vm.provision "shell", inline:<<-SHELL
			dnf clean
			dnf update -y
			dnf install httpd -y
			dnf install tor -y

			#set up the firewall 
			sudo firewall-cmd --permanent --zone=public --add-service=http
			sudo firewall-cmd --permanent --zone=public --add-service=https
			sudo firewall-cmd --reload

			echo "HiddenServiceDir /var/lib/tor/sitenadeep" >> /etc/tor/torrc
			echo "HiddenServicePort 80 127.0.0.1:80" >> /etc/tor/torrc 
			
		    mv index.html  /var/www/html/ #moving the page index 

			chmod 755 -R /var/www/html/

			systemctl start tor 
			systemctl start httpd

			cat /var/lib/tor/sitenadeep/hostname > /home/vagrant/onionAddress
	SHELL
	end 

	config.vm.provision "shell", inline:<<-SHELL
		dnf clean 
		dnf update -y
	SHELL
end
