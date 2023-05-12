---
layout: post
categories: unix
title : "Don't let non-root user run docker"
---
## Why we don't let non-root user run docker in CentOS, Fedora, RHEL

Awalnya cuma iseng kok gak bisa ya run docker dari user saya. Padahal sudah ditambahkan di group docker dan sudoers. Kemudian seperti biasanya ngangsu kaweruh pada simbah google kemudian bertemu dengan sensei[askbuntu.com][1] yang punya masalah persis seperti saya. Kemudian ada bacaan menarik dari [projectatomic][2]. Lalu baca-baca di beberapa komentar yang dibawahnya ternyata dari docker sendiri telah memberikan caranya, bisa dibaca di[sini][3]. Tapi ada beberapa hal yang menarik untuk dibahas sebenarnya. 

Tentunya yang menarik adalah tentang keamanan. Dari  artikel pertama yang dikhawatirkan dengan membuat docker group sebagai pemilik dan jika menambahkan user biasa tanpa sudo untuk mengakses docker socket secara langsung tidak akan ada catatan dan audit. Sehingga jika terjadi masalah yang disebabkan oleh user tidak ada catatannya. Namun docker juga sudah memberikan solusi dan peringatan tentang celah keamanan. Jika memberikan user akses ke docker. Sama-sama punya celah namun alasan yang pertama lebih punya risiko yang lebih banyak. Karena bisa menghapus config json jika user melakukan `docker login`.

Maka lebih aman jika menggunakan cara dari docker. Meskipun belum tahu juga apakah ini juga masuk kedalam sistem log yang ada di linux. Solusi yang pertama bagus dan perhatian tentang logging dan audit di Linux. Namun ada risiko keamanan yang terjadi. 

Untuk melakukan dengan cara docker. Pertama tambahkan group docker.
```
$ sudo groupadd docker
```
Kemudian tambahkan user ke docker group tadi.
```
$ sudo usermod -aG docker $USER
```
Setelah itu log out dan login lagi untuk memastikan perintah sudah diload sistem. Untuk pengecekan jalan docker run
```
$ docker run hello-world
```

Begitu adanya artikel ini. Jika ada kritik bisa ditambahkan. Saat tulisan ini ditulis belum ada dissqus. Mungkin akan saya tambahkan nanti. 
Selamat membaca.

[1]:https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo
[2]:https://www.projectatomic.io/blog/2015/08/why-we-dont-let-non-root-users-run-docker-in-centos-fedora-or-rhel/
[3]:https://docs.docker.com/engine/installation/linux/linux-postinstall/#configure-docker-to-start-on-boot
