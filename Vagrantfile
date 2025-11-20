Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos9s"

  config.vm.network "forwarded_port", guest: 80, host: 8888, auto_correct: true

  host_www = "C:/Users/alexs/Desktop/IHT/Lab1/www-content"
  config.vm.synced_folder host_www, "/vagrant", mount_options: ["dmode=755","fmode=644"]

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
  end

  config.vm.provision "shell", inline: <<-SHELL
    yum install -y epel-release
    yum install -y nginx
    setenforce 0
    sed -ie 's/^SELINUX=.*$/SELINUX=disabled/' /etc/selinux/config
    systemctl enable nginx
    systemctl disable firewalld
    systemctl stop firewalld

    cat > /etc/nginx/conf.d/vagrant.conf <<EOF
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;
    root /var/www/html;
    index index.html;
}
EOF

    cp -r /vagrant/* /var/www/html/
    systemctl restart nginx
  SHELL
end