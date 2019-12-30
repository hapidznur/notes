---
author: hapidznur
categories:
  - journal
image: spools.jpg
tags:
  - documentation
  - docker
title: Add Network Name and IP Address to container in Compose
date: 2019-05-20T14:15:59-06:00

url: /2019/05/20/Add-network-name-and-ip-static-compose/
---


## Compose

Note: Membutuhkan pengetahuan tentang docker dan docker-compose.

Terakhir kali menggunakan database selalu disediakan oleh framework jadi jarang mengkonfigurasikan IP dan cara mengkoneksikannya. 
Nah karena sekarang sedang proses pindah enviroment menjadi full docker jadi sedikit ada tantangan di menghubungkan jaringannya. 
Selain itu kedepan kayaknya butuh menghubungkan antar network dalam docker sehingga bisa dipisahkan tiap enviroment yang digunakan. 

## Menambahkan nama jaringan
Biar lebih gampang docker sudah memberikan nama pada host namun belum pada network. Update terbaru pada versi compose 3.5 yang bisa memberikan nama sesuai kebutuhan karena sebelumnya digenerate dari compose. 

Contoh config:

```yaml
version: "3.5"
networks:
  frontend:
    name: custom_frontend
    driver: custom-driver-1
```



## IP Static pada container 

Sebenarnya dalam best practice ip static untuk container tidak disarankan. Namun untuk enviroment yang saya butuhkan lebih gampang mengunakan ip static daripada mengunakan nama host. Seinget saya penambahan ip static sudah bisa dilakukan dari versi 2. Namun karena pada konfigurasi ini saya mengunakan versi 3.5.


```yaml
version: '3.5'
services:
  web:
    networks:
      db-network:
        ipv4_address: 10.20.0.3
networks:
  frontend:
    name: custom_frontend
    driver: custom-driver-1
    ipam:
      config:
        - subnet: 10.20.0.0/16

```

Sekian.

Sumber
[1]: https://docs.docker.com/compose/networking/

