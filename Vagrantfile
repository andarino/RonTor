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
			
		    echo "            
<html>
________00000000000___________000000000000_________<br>
______00000000_____00000___000000_____0000000______<br>
____0000000_____________000______________00000_____<br>
___0000000_______________0_________________0000____<br>
__000000____________________________________0000___<br>
__00000_____________________________________ 0000__<br>
_00000___________VAGRANT AND TOR____________00000__<br>
_00000_____________________________________000000__<br>
__000000_________________________________0000000___<br>
___0000000______________________________0000000____<br>
_____000000____________________________000000______<br>
_______000000________________________000000________<br>
__________00000_____________________0000___________<br>
_____________0000_________________0000_____________<br>
_______________0000_____________000________________<br>
_________________000_________000___________________<br>
_________________ __000_____00_____________________<br>
______________________00__00_______________________<br>
________________________00_________________________<br>

</html>" > /var/www/html/index.html

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
