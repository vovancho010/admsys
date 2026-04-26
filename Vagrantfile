ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro' # Для того чтобы качать образы (box) для развертывания ОС машин.

Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/bionic64" # определили образа для всех машин 
	config.hostmanager.enabled = true # активирует плагин vagrant-hostmanager он втоматически управляет файлами `hosts` на  виртуальных машинах
	config.vm.provider "virtualbox" do |vb|
		vb.memory = 512
		vb.customize ["modifyvm", :id, "--audio", "none"]
	end
	
	# Отключаем автоматическую генерацию ключей Vagrant
    config.ssh.insert_key = false
    # Указываем путь к нашему приватному и публичному ключу
    config.ssh.private_key_path = "./keys/id_1"
	config.ssh.public_key_path = "./keys/id_1.pub"
	
	
# Первая машина Nginx
	config.vm.define "nginx_balanser" do |nginxbalans|
		nginxbalans.vm.hostname = "nginx_balanser"
		nginxbalans.vm.network "private_network", ip: "192.168.3.10"
		nginxbalans.vm.network "forwarded_port", guest: 80, host:8080
		
		#это запуск конкретного ansible playbook для этой машины 
		#nginxbalans.vm.provision "ansible" do |ansible|
			#ansible.playbook = "ansible/nginx_playbook.yml"
			#ansible.verbose = "v" # вывод действий ansible
		#end
		# можно этот участок для каждой машины прописать но лучше вынести его как общий для всех.
		#nginxbalans.vm. provider "virtualbox" do |vb|
			#vb.memory = 512
			#vb.customize ["modifyvm", :id, "--audio", "none"] # отключили звуковую карту для VM что на тратить ресурсы. 
		#end
	end 

# Вторая машина Web-сервис_1
	config.vm.define "web1" do |web_1|
		web_1.vm.hostname = "web1"
		web_1.vm.network "private_network", ip: "192.168.3.11"
	end
	
# Третья машина Web-сервис_2
	config.vm.define "web2" do |web_2|
		web_2.vm.hostname = "web2"
		web_2.vm.network "private_network", ip: "192.168.3.12"
	end 
	
# Четвертая машина PostgresQL 
	config.vm.define "database" do |postgres|
		postgres.vm.hostname = "database"
		postgres.vm.network "private_network", ip: "192.168.3.13"
	end 


# это  использовать один и тотже ansible playbook для всех машин. но можно и для каждой в отдельности как это сделано в коментарии у первой машины. 
	config.vm.provision "ansible" do |ansible|
		ansible.playbook = "ansible/playbook.yml"
		ansible.inventory_path = "ansible/hosts"
		ansible.limit = "all"
		ansible.verbose = "v"
	end
		
end 
