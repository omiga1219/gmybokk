#### **查看已开放的端口**

```
firewall-cmd --list-ports
```

#### **开放端口**

（开放后需要要重启防火墙才生效）

firewall-cmd --zone=public --add-port=3338/tcp --permanent

#### **重启防火墙**

firewall-cmd --reload

#### **关闭端口**

（关闭后需要要重启防火墙才生效）

firewall-cmd --zone=public --remove-port=3338/tcp --permanent

#### 开机启动防火墙

systemctl enable firewalld

#### **开启防火墙**

systemctl start firewalld

#### 禁止**防火墙**开机启动

systemctl disable firewalld

#### 停止防火墙

systemctl stop firewalld