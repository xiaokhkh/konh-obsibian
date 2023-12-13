

docker 和 虚拟机linux不兼容
1，如果用docker，
第一步：在控制面板中勾选Hyper -v
第二步：在cmd，以管理员身份运行：
 
bcdedit /set hypervisorlaunchtype auto
第三步;重启
 
2，开启虚拟机linux
第一步：在控制面板中取消勾选Hyper -v
第二步：在cmd中，以管理员身份运行：
bcdedit /set hypervisorlaunchtype off
第三步;重启