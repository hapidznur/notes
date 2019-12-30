---
author: hapidznur
categories:
  - journal
tags:
  - documentation
  - docker
  - working
title: Connecting container in multiple networks in docker
date: 2019-05-20T14:15:59-06:00

url: /2019/05/20/connecting-networks-in-docker/
---




## Goal

Docker telah menyediakan fitur untuk menghubungkan banyak container dengan banyak jaringan. Bermanfaat sekali untuk pembuatan datacenter. Untuk keperluan mini datacenter dilaptop saya pribadi juga butuh.

## Step
Pertama kali yang disiapkan adalah docker tentu saja. Untuk dokumentasi ini saya memkonversi dari halaman sumber docker. 

Setiap container yang dibuat pertama kali hanya bisa mendapatkan 1 jaringan. Untuk menghubungkan antar container yang berbeda jaringan dapat dilakukan dengan `docker network connect`. Semudah itu saja sebenarnya. 

## Step 1

Buat Network dulu. 

```bash
$ docker network create blueocean
$ docker network create waterpool
```

## Step 2
Jalankan docker containernya. Kalau belum ada otomatis download. 

```bash
$ docker run -itd --net waterpool --name c1 busybox sh
```

## Step 3 
Tambahkan network `blueocean`. 

```bash
$ docker network connect blueocean c1
```

## Step 4
Cek IP address

```bash
$ docker exec -it c1 sh
/ # ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:1D:00:02
          inet addr:172.29.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe1d:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:16 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1296 (1.2 KiB)  TX bytes:648 (648.0 B)

eth1      Link encap:Ethernet  HWaddr 02:42:AC:1E:00:02
          inet addr:172.30.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe1e:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:16 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1296 (1.2 KiB)  TX bytes:648 (648.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

/ # ip route
default via 172.29.0.1 dev eth0
172.29.0.0/16 dev eth0  src 172.29.0.2
172.30.0.0/16 dev eth1  src 172.30.0.2
```

Hasilnya adalah container tersebut bisa diakses oleh container didalam blueocean dengan ip address ataupun hostname. 

Sumber:
[1]: https://runnable.com/docker/basic-docker-networking
[2]: https://success.docker.com/article/multiple-docker-networks
