---
categories:
  - unix
title: Install Docker di Fedora 25
date: 2018-03-10T14:15:59-06:00
url: /2018/03/10/install-docker/
---


## Instal Docker di Fedora 25

Untuk lebih mengenal docker silahkan kunjungi situs resmi [docker][docker].
Ini hanya dokumentasi saya saja. Mohon maaf jika kurang lengkap.

- Hapus semua docker lama yang sudah terinstal jika ada.
```bash
$ sudo dnf remove docker \\
                  docker-common \\
                  docker-selinux \\
                  docker-engine-selinux \\
                  docker-engine
```
- Langkah kedua tambahkan dnf plugins manager
```bash
$ sudo dnf -y dnf-plugins-core
```
- Kemudian tambahkan repository docker
```bash
$ sudo dnf config-manager https://download.docker.com/linux/fedora/docker-ce.repo
```
- Kemudian install docker
```bash
$ sudo dnf install docker-ce
```

Oke docker selesai diinstal. 

[docker]:https://www.docker.com
