# vagrant 使用

## 常用命令

命令|作用
----|----
vagrant box add |添加box的操作
vagrant init    |初始化box的操作，会生成vagrant的配置文件Vagrantfile
vagrant up  |启动本地环境
vagrant ssh |通过 ssh 登录本地环境所在虚拟机
vagrant halt    |关闭本地环境
vagrant suspend |暂停本地环境
vagrant resume  |恢复本地环境
vagrant reload  |修改了 Vagrantfile 后，使之生效（相当于先 halt，再 up）
vagrant destroy |彻底移除本地环境
vagrant box list    |显示当前已经添加的box列表
vagrant box remove  |删除相应的box
vagrant package |打包命令，可以把当前的运行的虚拟机环境进行打包
vagrant plugin  |用于安装卸载插件
vagrant status  |获取当前虚拟机的状态
vagrant global-status   |显示当前用户Vagrant的所有环境状态
