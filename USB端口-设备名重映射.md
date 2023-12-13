在Ubuntu里，当多个外接设备接入USB时，其挂载的USB端口并不是固定的，给一些USB通信程序的自动化运行带来不便。常见的解决方案是对这些USB端口进行设备名重映射，即通过识别USB端口上的设备唯一性信息，适配到用户自定义的名称上。

具体做法
1、终端输入命令lsusb，查看现有电脑USB口连接情况。

显示列表如下图示意：

![[Pasted image 20231102175759.png]]

2、USB口插上设备后，终端再次输入命令lsusb，对比确认新增设备情况。

重点关注ID后的8位：XXXX:XXXX。例如，实验中通过一个FT232的USB转TTL模块新插入了一个北醒TOF测距传感器，可以从列表中找到相应信息（注意，只会显示直接连接的转换模块信息）：
![[Pasted image 20231102175804.png]]
 

3、终端输入命令，转到rules配置文件夹下，并在该目录下新建测距传感器对应的rules文件：

cd /etc/udev/rules.d/
sudo vim tfmini_s.rules
输入以下内容，用于重映射usb设备名：

KERNEL=="ttyUSB*", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001",  ATTRS{serial}=="0001", MODE:="0777", SYMLINK+="tfmini_s"

说明：

KERNEL是内核固定名称，这里统一是“ttyUSB*”；
MODE是节点权限，通常改为“0777”，表示可读写运行；
SYMLINK是符号连接，即重映射的节点名称；
ATTRS是设备厂商的唯一标识，包括3部分：idVendor和idProduct正好组成上面通过lsusb查找到的设备ID，serial是端口号，可以通过输入以下命令：
udevadm info -a -p $(udevadm info -q path -n /dev/ttyUSB0)
注意：上面命令中的“/dev/ttyUSB0”根据设备挂载情况自行调整。返回结果中可以找到对应信息：

![[Pasted image 20231102175824.png]]

如果挂载多个同类设备，就需要通过serial来识别。


4、保存文件退出后，输入以下命令重载规则并重启dev：

sudo service udev reload
sudo service udev restart


5、重新插拔下设备，输入以下命令确认设备名重映射是否成功：

ll /dev | grep ttyUSB
如下所示，可以看到设备名挂载到USB1端口：

![[Pasted image 20231102175830.png]]

后续再使用该设备进行USB通信时，就可以直接使用“/dev/tfmini_s”来替代“/dev/ttyUSB*”。