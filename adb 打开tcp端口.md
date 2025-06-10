```shell
su # 获取root权限 
mount -o remount,rw /system # 重新挂载/system分区为可写 
echo "service.adb.tcp.port=5555" >> /system/build.prop # 在build.prop文件尾部增加一行 
mount -o remount,ro /system # 重新挂载/system分区为只读
```


``` 设置某个应用为主屏幕应用
adb shell cmd package set-home-activity com.wisdom_starter/.MainActivity
```