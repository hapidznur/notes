---
title: 'Install Docker di Fedora 25'
layout: post
categories: unix
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

