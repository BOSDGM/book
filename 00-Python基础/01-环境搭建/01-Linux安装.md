# 1. 安装

在执行安装命令之前, 先进行对应的依赖安装. 

安装中出现异常,  **注意**:

* 必须进行缓存清理`make clean`
* 必须从头开始.

Python源码编译(在线安装不做解释)安装正常流程(不报错情况)如下

## 1.1 配置安装目录

```python
mkdir /usr/local/python
mkdir /usr/local/Packages
mv Python-3.7.2.tgz /usr/local/Packages
cd /usr/local/Packages
tar -zxf Python-3.7.2.tgz
cd Python-3.7.2
```

## 1.2 安装Python

```python
./configure --prefix=/usr/local/Python/python37 --enable-shared --enable-optimizations # --with-openssl=/usr/local/openssl  指定安装后的openssl的目录

make -j4

make install
```

## 1.3 配置环境变量

```python
cp libpython*.so.1.0 /usr/lib
cp libpython*.so.1.0 /usr/lib64
cp libpython*.so.1.0 /usr/local/lib 
cp libpython*.so.1.0 /usr/local/lib64

# vim /etc/profile
# 增加
# PATH=$PATH:/usr/local/python/python37/bin

source /etc/profile
```

## 1.4 测试

```python
python3.7 -c "import ssl"
python3.7 -c "import ctypes"
python3.7 -c "import zlib"
python3.7 -c "import readline"
python3.7 -c "import lzma"
python3.7 -c "import uuid"
# python3.7 -c "import tk" 这个需要安装对应模块才能测试, 可以不测
# python3.7 -c "import sqlite" 这个需要安装对应模块才能测试, 可以不测
```



## 2. Python依赖

python需要的全部依赖. 正常情况下, 安装完成, 是不会出现异常的

### 2.1 Rehat

```python
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite sqlite-devel readline-devel tk tk-devel gdbm gdbm-devel db4-devel libpcap-devel lzma xz xz-devel libuuid-devel libffi-devel
```

## 2.2 Ubuntu



# 3. 依赖处理

## 3.1 openssl

### 3.1.1 异常

* Python安装异常

  Python3.7.2 要求openssl/LibreSSL版本不得低于 1.0.2, 1.1, 或者LireSSL不得低于2.6.4

  ```python
  Python requires an OpenSSL 1.0.2 or 1.1 compatible libssl with X509_VERIFY_PARAM_set1_host().
  LibreSSL 2.6.4 and earlier do not provide the necessary APIs, https://github.com/libressl-portable/portable/issues/381
  ```
  
* pip使用异常

  缺少openssl

  ```python
  Ignoring ensurepip failure:pip required SSL/TLS
  ```

### 3.1.2 Redhat

安装并指定需要的openssl/LibreSSL版本

本次以openssl为例进行安装

* 下载

  ```python
  wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz
  mv openssl-1.1.1g.tar.gz /usr/local/Packages
  cd /usr/local/Packages
  tar -zxf openssl-1.1.1g.tar.gz
  ```

* 安装

  ```python
  cd openssl-1.1.1g
  ./config --prefix=/usr/local/openssl
  ./config -t
  make depend
  make
  make test
  make install
  ```

  

* 修改Python编译时的参数注意

  1. 增加Python配置项`./configure --with-openssl=/usr/local/openssl`

     ```python
     --with-openssl项必须为yes
     ```

     

  2. 执行`make`后, 检查`Modules/Setup.dist`, 确保以下三行没有注释:

     ```python
     SSL=/usr/local
     _ssl _ssl.c \
             -DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
             -L$(SSL)/lib -lssl -lcrypto
     ```

     

  

## 3.2 libffi

### 3.2.2 Redhat

* 下载

  ```python
  wget ftp://sourceware.org/pub/libffi/libffi-3.2.1.tar.gz
  mv libffi-3.2.1.tar.gz /usr/local/Packages
  cd /usr/local/Packages
  tar -zxf libffi-3.2.1.tar.gz
  ```

* 安装

  ```python
  cd libffi-3.2.1
  ./configure
  make
  make install
  ```

* 参数配置

  ```python
  export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH
  # 查找find / -name libffi.so.6, 将结果配置到LD_LIBRARY_PATH中
  export LD_LIBRARY_PATH=/usr/local/lib64
  ```

##  3.3 GCC

### 3.3.1 异常

* 未安装gcc

  ```python
  configure:error: no accetable cc found in $PATH
  ```

### 3.3.2 Redhat

![gcc rpm包](./01-Python.assets/gcc_rpm.tar.gz)

```python
rpm -ivh lib64gmp3-4.3.1-1mdv2010.0.x86_64.rpm
rpm -ivh ppl-0.10.2-11.el6.x86_64.rpm
rpm -ivh cloog-ppl-0.15.7-1.2.el6.x86_64.rpm
rpm -ivh mpfr-2.4.1-6.el6.x86_64.rpm
rpm -ivh cpp-4.4.7-4.el6.x86_64.rpm --force
rpm -ivh kernel-headers-2.6.32-431.el6.x86_64.rpm
rpm -ivh glibc-headers-2.12-1.132.el6.x86_64.rpm --nodeps --force
rpm -ivh glibc-devel-2.12-1.132.el6.x86_64.rpm --force --nodeps
rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm --force --nodeps
rpm -ivh libstdc++-devel-4.4.7-4.el6.x86_64.rpm --force --nodeps
rpm -ivh gcc-c++-4.4.7-4.el6.x86_64.rpm --force --nodeps
rpm -e --nodeps keyutils-libs-1.4-4.el6.x86_64
rpm -ivh keyutils-libs-1.4-5.el6.x86_64.rpm
rpm -ivh keyutils-libs-devel-1.4-5.el6.x86_64.rpm 
rpm -ivh libsepol-devel-2.0.41-4.el6.x86_64.rpm 
rpm -e --nodeps libselinux-utils-2.0.94-5.3.el6_4.1.x86_64
rpm -Uvh libselinux-2.0.94-5.8.el6.x86_64.rpm
rpm -ivh libselinux-devel-2.0.94-5.8.el6.x86_64.rpm
rpm -e --nodeps krb5-libs-1.10.3-10.el6_4.6.x86_64
rpm -ivh krb5-libs-1.10.3-42.el6.x86_64.rpm
rpm -e --nodeps libcom_err-1.41.12-18.el6.x86_64
rpm -ivh libcom_err-1.41.12-22.el6.x86_64.rpm 
rpm -ivh libcom_err-devel-1.41.12-22.el6.x86_64.rpm
rpm -ivh krb5-devel-1.10.3-42.el6.x86_64.rpm
rpm -ivh zlib-devel-1.2.3-29.el6.x86_64.rpm 
rpm -e --nodeps openssl-1.0.1e-15.el6.x86_64
rpm -ivh openssl-1.0.1e-42.el6.x86_64.rpm 
rpm -ivh openssl-devel-1.0.1e-42.el6.x86_64.rpm
```



## 3.4 Sqlite

### 3.4.1 异常

* sqlite版本低

  需要更新版本

  ```python
  raise ImproperlyConfigured('SQLite 3.8.3 or later is required (found %s).' % Database.sqlite_version)
  django.core.exceptions.ImproperlyConfigured: SQLite 3.8.3 or later is required (found 3.6.20).
  ```

### 3.4.2 Redhat

* 下载

  ```python
  https://www.sqlite.org/2020/sqlite-autoconf-3320200.tar.gz
  mv sqlite-autoconf-3320200.tar.gz /usr/local/Packages
  cd /usr/local/Packages
  tar -zxf sqlite-autoconf-3320200.tar.gz
  ```

* 安装

  ```python
  cd /usr/local/Packages/sqlite-autoconf-3280000/
  ./configure --prefix=/usr/local/sqlite
  mak -j4
  make install
  ```

* 版本替换

  ```python
  mv /usr/bin/sqlite3 /usr/bin/sqlite3_old
  cd /usr/local/sqlite/bin/
  ln -s sqlite3 /usr/bin/sqlite3
  
  vim /etc/profile
  export LD_LIBRARY_PATH=/usr/local/sqlite/lib
  source /etc/profile
  ```









