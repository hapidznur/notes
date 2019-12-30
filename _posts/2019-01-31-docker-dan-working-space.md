---
layout: post
title: "Docker as Working Enviroment"
author: "hapidznur"
categories: journal
tags: [documentation, docker]
image: docker.png
---


Tujuannya sebenarnya mengurangi konflik yang terjadi setiap kali mau eksperimen sesuatu. Misalnya mau coba-coba untuk python tapi fokus data science namun ternyata bentrok dengan dependencies yang buuat ngoding ERPNext. Nah akhirnya salah satu [temen](elfarqy.net) ngasih saran buat misahin tiap working enviroment dengan docker. 

Nah tulisan ini adalah dokumentasi dari working yang telah saya gunakan. Selain itu juga nanti ada beberapa tulisan mengenai docker yang jadi dokumentasi dari config yang tiap hari digunakan. 

Selain Docker ada Docker Compose yang digunakan untuk membuat enviroment variable. Docker Compose ini digunakan untuk mendefinisikan perintah dan kebutuhan dari container yang akan dibuat. Selain mempersingkat waktu untuk eksekusi juga lebih hemat waktu dalam melakukan konfigurasi.

## Contoh config
```yaml
version: '3.2'
services:
  blog:
    image: hapidznur/jekyll
    container_name: nednight
    ports:
      8000:8000
    networks:
      - blog
networks:
  blog:
```
Konfigurasi diatas penggunaan konfigurasi docker-compose. Untuk sintaks lain dapat dilihat langsung pada dokumentasi dari [docker-compose](1). 

## Dokumentasi Config
Struktur direktori berikut digunakan untuk ngoding yang lebih cepet. Untuk biar lebih cepat juga menggunakan `ENV` dan `ARGS` namun belum saya bahas pada tulisan ini.
```
\ ComposeShip
  -\ compose
    - compose-database.yml
    - compose-python.yml
    - compose-automation.yml
    - compose-go.yml
    - compose-service.yml
    - etc
  -\ config
    -\ python
    -\ go
    -\ service
    -\ etc..
  - .env.example
  - README.md
  - LICENSE
```

[1]: https://docs.docker.com/get-started/
[2]: https://www.github.com/hapidznur/ComposeShip


