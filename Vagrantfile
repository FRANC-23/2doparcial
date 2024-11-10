# Vagrantfile

Vagrant.configure("2") do |config|
  
    # Configuración de web1
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/bionic64"
      web1.vm.hostname = "web1"
      web1.vm.network "private_network", ip: "192.168.56.4"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        echo "Servidor web1" > /var/www/html/index.html
      SHELL
    end
    
    # Configuración de web2
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/bionic64"
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", ip: "192.168.56.5"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        echo "Servidor web2" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración del balanceador de carga
    config.vm.define "loadbalancer" do |loadbalancer|
      loadbalancer.vm.box = "ubuntu/bionic64"
      loadbalancer.vm.hostname = "loadbalancer"
      loadbalancer.vm.network "private_network", ip: "192.168.56.6"
      loadbalancer.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      loadbalancer.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        # Configuración de Nginx para balancear las peticiones entre web1 y web2
        cat <<EOF > /etc/nginx/conf.d/load_balancer.conf
  upstream backend {
      server 192.168.50.10;
      server 192.168.50.11;
  }
  
  server {
      listen 80;
  
      location / {
          proxy_pass http://backend;
      }
  }
  EOF
        systemctl restart nginx
      SHELL
    end
  end
  