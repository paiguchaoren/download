
## 一、coreutils--给Linux中的cp和mv命令中添加进度条的高级拷贝

- 安装“Advanced Copy”补丁，将进度条添加到Linux的cp和mv命令中




```javascript
wget https://github.com/paiguchaoren/download/raw/master/download/vps/centos/coreutils-9.1.tar.gz

tar -zxvf coreutils-9.1.tar.gz 

cd coreutils-9.1/ 
```

- 使用以下命令下载Advanced Copy补丁: 

```javascript
wget https://github.com/paiguchaoren/download/raw/master/download/vps/centos/advcpmv-0.9-9.1.patch  
```


- 最后，通过逐个运行以下命令应用补丁: 

```javascript
patch -p1 -i advcpmv-0.9-9.1.patch  
./configure 
make 
```

- 现在将在coreuths -8.32/src文件夹中创建两个新的补丁二进制文件cp和mv。只需像下面这样将它们复制到你的$PATH: 

```javascript
sudo cp src/cp /usr/local/bin/cp  
[sudo] linuxmi 的密码：  
sudo cp src/mv /usr/local/bin/mv  
```


- 就这样。cp和mv命令现在有了进度条功能。

- 当你在复制或移动文件和目录时想要一个进度条，只需添加 -g 标签，如下所示: 

```javascript
cp -g /home/linuxmi/Fedora-Silverblue-ostree-x86_64-32-1.6.iso /home/linuxmi/www.linuxmi.com/ 
```

- 或者使用 --progress-bar 标签: 

```javascript
cp --progress-bar /home/linuxmi/Fedora-Silverblue-ostree-x86_64-32-1.6.iso /home/linuxmi/www.linuxmi.com/ 
```

- 使用mv命令移动目录:

```javascript
mv -g directory1/ directory2/ 
```




## 二、宝塔面板降级教程7.7.0


### 宝塔面板降级教程7.7.0

- 下载安装离线升级包

```
wget https://github.com/paiguchaoren/download/raw/master/download/vps/centos/LinuxPanel-7.7.0.zip
unzip LinuxPanel-7.7.0.zip
cd /root/panel
bash update.sh
```

- 降级完成后建议开启离线模式：面板设置 – 离线模式。离线模式只能保证宝塔主程序不主动联网更新。如果由宝塔下发强制更新，还是一样会被更新的。


1.  删除文件bind.pl
```
rm -rf  /www/server/panel/data/bind.pl
```
2.  清除index.js中代码，SSH中运行。清除后需使用无痕模式登录一次。
```
sed -i "s|bind_user == 'True'|bind_user == 'XXXX'|" /www/server/panel/BTPanel/static/js/index.js
```


## 三、frp

1. 创建服务端


```
 wget https://github.com/paiguchaoren/download/raw/master/download/vps/centos/frp_0.38.0_linux_amd64.tar.gz
 tar -zxvf frp_0.38.0_linux_amd64.tar.gz
 mv frp_0.38.0_linux_amd64 /etc/frp
 cd /etc/frp
 vi frps.ini
```

- 编辑配置文件


```
[common]
bind_addr = 0.0.0.0        #改为允许所有IP连接
bind_port = 7000
dashboard_port = 7500
dashboard_user = user      #配置网页访问用户名
dashboard_pwd = passowrd   #配置网页访问密码
log_level = info
log_max_days = 3
token = 123456789         #配置通道连接密码
max_pool_count = 5
max_ports_per_client = 0
tcp_mux = true
disable_log_color = false
log_file = ./frps.log
```

- 设置frps服务并启动

```
cd /etc/frp
cp frps /usr/bin                                      -- 将frps服务复制到/usr/bin中
cp systemd/frps.service /usr/lib/systemd/system/      -- 将sustemd/frps.services 服务注册配置信息迁移到/usr/lib/systemd/system/
systemctl enable frps                                 -- 设置开机自启动
systemctl start frps                                  -- 启动frps服务


systemctl status frps                                 -- 查看启动日志
systemctl restart frps                                -- 重启服务
systemctl stop frps                                   -- 关闭服务
```

可以输入 http://公网IP:7500 访问管理后台页面

![服务端图示](https://github.com/paiguchaoren/download/blob/master/download/vps/centos/images/frps.jpg)




2. 客户端

- 下载地址同客户端，解压后找到其中frpc.ini 文件，进行以下修改

```
[common]
#指定服务端IP地址及端口
server_addr = 公网IP   #服务端公网IP地址，需要能够连通
server_port = 7000    #对应服务端设定bind_port
token = 123456789     #对应服务端设定的token

[web]
type = tcp
local_ip=127.0.0.1
local_port = 3389
remote_port=7601
```

启动frpc服务，

```
./frpc -c ./frpc.ini 
```

或者直接用宝塔控制面板

![客户端图示](https://github.com/paiguchaoren/download/blob/master/download/vps/centos/images/frpc.jpg)


- 验证是否穿透成功

直接浏览器中输入 http://公网IP:remote_port 设定端口  能够正常访问我们开发的服务即完成。


- 其他：查看日志信息

```
$ sudo systemctl status frpc
```






## 四、其余

持续更新中...