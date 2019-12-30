---
author: hapidznur
categories:
  - unix
tags:
  - documentation
  - centos
  - brimir
title: Install Brimir on Centos
date: 2018-03-23T14:15:59-06:00

url: /2018/03/23/Install-Phusion-Passenger/
---



Install keyserver dulu ya buat RVM nya
```
$ sudo gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
```

Ambil dulu rvm-nya
```
$ \curl -sSL https://get.rvm.io | sudo bash -s stable
```
Mari install ruby-nya
```
$ rvm install 2.3.3
```

Ceknya gimana?
```
$ rvm listrvm rubies=* ruby-2.3.3 [ x86_64 ]
```

Pasti sudah punya mysql kan? Langsung saja ya.
Mari buat database khusus buat brimir.
```
$ mysql -u root # masuk mysql dulu

mysql> CREATE DATABASE brimir;
```
Buat usernya khusus brimir
```
mysql> GRANT ALL PRIVILEGES ON `brimir`.* TO 'user_brimir'@'localhost' IDENTIFIED BY 'password';
mysql> FLUSH PRIVILEGES;
```


