# 1. yum安装

## 1.1 yum安装

### 1.1.1 删除yum

```bash
rpm -qa|grep yum|xargs rpm -e --nodeps
```

### 1.2.1 下载安装yum

* 下载

  注意使用的版本, 当前为6.

  ```bash
  wget http://mirrors.aliyun.com/centos/6/os/x86_64/Packages/rpm-4.8.0-59.el6.x86_64.rpm &&
  wget http://mirrors.aliyun.com/centos/6/os/x86_64/Packages/yum-metadata-parser-1.1.2-16.el6.x86_64.rpm &&
  wget http://mirrors.aliyun.com/centos/6/os/x86_64/Packages/python-urlgrabber-3.9.1-11.el6.noarch.rpm &&
  wget http://mirrors.aliyun.com/centos/6/os/x86_64/Packages/yum-3.2.29-81.el6.centos.noarch.rpm &&
  wget http://mirrors.aliyun.com/centos/6/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.30-41.el6.noarch.rpm
  ```

* 安装

  ```bash
  rpm -ivh *.rpm --force --nodeps  # 强制批量安装
  ```

## 1.2 Rehat激活

未激活的Redhat, 需要重新安装yum, 否则不能安装软件. 也可以直接激活, 但是不建议, 官方的软件包太少了.

激活后换源是不支持的.

### 1.2.1 开发者账号注册

访问下列网站完成注册

```bash
https://developers.redhat.com/auth/realms/rhd/protocol/openid-connect/registrations?client_id=web&redirect_uri=http%3A%2F%2Fdevelopers.redhat.com%2Fblog%2F2014%2F06%2F26%2Frhel-7-is-for-developers%2F&state=8bc8e6d2-7dea-42c2-96da-c5e2e5755fca&nonce=eab2e0c7-b41d-44a9-99ad-0c2dbd5170a9&response_mode=fragment&response_type=code
```

### 1.2.2 账号登录激活

```bash
[root@localhost gcc-4.8.5]# subscription-manager register --username=565956231@qq.com --password=wdlRE4911? --auto-attach
The system has been registered with ID: 1b6c59d6-fb26-454d-ab37-fe14837f261c 
Installed Product Current Status:
Product Name: Red Hat Enterprise Linux Server
Status: Subscribed
```

# 2. yum换源

## 2.1 在线源

源文件位置: /etc/yum.repos.d/*.repo

1. 删除源文件

   ```bash
   cd /etc/yum.repos.d
   rm -rf *.repo
   ```

2. 下载需要使用的源

   ```bash
   wget http://mirrors.aliyun.com/repo/Centos-6.repo  # 阿里云
   wget http://mirrors.163.com/.help/CentOS6-Base-163.repo # 网易
   ```

3. 修改当前系统使用的源版本

   ```bash
   vim Centos-6.repo  #进入vim进行批量替换
   :%s/$releasever/6/g
   ```

4. 清理缓存, 即可使用

   ```bash
   yum clean all
   yum makecache
   ```

   

## 2.2 挂载ISO盘

### 2.2.1 开机自动加载

```bash
vim /etc/fstab
```

追加内容

```bash
/dev/sr0		/mnt		iso9660     defaults   0   0  # 或者
/dev/cdrom              /mnt                    iso9660 defaults        0 0
```

* /dev/cdrom: 需要挂载的盘符
* /mnt:  挂载的位置
* iso9660: iso盘格式
* defaults: 控制权限等问题
* 0  0: 是否进行开启检测  , 0表示不检测

### 2.2.2 挂载ISO盘

```bash
mount -a  # 挂载
umount /mnt/  # 卸载挂载
```

查看挂载内容

```bash
ls /mnt/
```

![image-20200710231223206](image/04-yum%E6%BA%90%E9%85%8D%E7%BD%AE/image-20200710231223206.png)

### 2.2.3 使用ISO盘

1. 清空源

   ```bash
   cd /etc/yum.repos.d/
   rm -rf *
   ```

2. 配置源

   ```bash
   vim test.repo
   ```

   增加如下内容

   ```bash
   [Redhat]
   name=Redhat-Server
   baseurl=file:///mnt
   enabled=1
   gpgcheck=0
   ```

   * name: 服务名称
   * baseurl: 指定使用源位置
   * enabled: 是否启用
   * gpgcheck: 是否检查

3. 清空缓存, 并使用

   ```bash
   yum clean all
   yum install python-pip
   ```

   