# linux学习

## 用户

### 创建新用户

```
useradd omiga_test
```

### 创建新用户的密码

``` 
passwd omiga_test
```

### 删除用户

```shell
userdel omiga_test
```

### 给用户添加sudo权限

```
vim /etc/sudoers

方法一：修改 /etc/sudoers 文件，找到下面一行，把前面的注释（#）去掉
Allows people in group wheel to run all commands
%wheel ALL=(ALL) ALL
然后修改用户，使其属于root组（wheel），命令如下：
usermod -g root ithing
修改完毕，现在可以用tommy帐号登录，然后用命令 su - ，即可获得root权限进行操作。

方法二：修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：
Allow root to run any commands anywhere
root ALL=(ALL) ALL
ithing ALL=(ALL) ALL
```




## 给用户添加组

### 创建组

```
groupadd docker
```

### 查看组信息

```
cat /etc/group
```

### 将用户添加到组中

```
gpasswd -a omiga_test docker

重启docker服务，omiga_test再重新登录就可以使用docker了
systemctl restart docker
```

