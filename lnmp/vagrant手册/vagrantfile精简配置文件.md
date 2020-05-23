文件名称:Vagrantfile

```
Vagrant.configure("2") do |config|
  config.vm.box = "0518"
  config.vm.network "forwarded_port", guest: 80, host: 1237
  # 配置网络信息
  config.vm.network "private_network", ip: "192.168.33.22"
  #自动同步的目录
  config.vm.synced_folder "E:/PHP/php", "/data/web/", owner: "www", group: "www"
   config.vm.provider "virtualbox" do |vb|
     vb.memory = "4096"
   end
end

```

