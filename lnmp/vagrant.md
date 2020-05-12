以安装php7&phalcon3.4扩展的box文件

可惜 我下载不下来

https://app.vagrantup.com/ryomo/boxes/phalcon3/versions/1.0.5



https://vagrantcloud.com/ryomo/boxes/phalcon3/versions/1.0.5/providers/virtualbox.box



centerOs7纯净系统的box

https://app.vagrantup.com/centos/boxes/7/versions/1905.1

https://vagrantcloud.com/centos/boxes/7/versions/1905.1/providers/virtualbox.box





### vagrant的命令详解



#### 第一步 添加虚拟机

vagrant box add 

vagrant box add v73 D:\soft\Vagrantfile\centos7_64.box

v73 是虚拟机的名字

后面的是系统box文件的路径

#### 第二步  初始化虚拟机

vagrant init 初始化box的操作，会生成vagrant的配置文件Vagrantfile

### 第三步 修改配置文件Vagrantfile

请看vagrantfile备份.md

#### 第四步 启动虚拟机

vagrant up 启动本地环境

#### 第五步 进入虚拟机

vagrant ssh 通过 ssh 登录本地环境所在虚拟机





vagrant halt 关闭本地环境
vagrant suspend 暂停本地环境
vagrant resume 恢复本地环境
vagrant reload 修改了 Vagrantfile 后，使之生效（相当于先 halt，再 up）
vagrant destroy 彻底移除本地环境

显示当前已经添加的box列表  vagrant box list 
vagrant box remove 删除相应的box
vagrant package 打包命令，可以把当前的运行的虚拟机环境进行打包
vagrant plugin 用于安装卸载插件
vagrant status 获取当前虚拟机的状态
vagrant global-status 显示当前用户Vagrant的所有环境状态