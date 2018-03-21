---
layout: post
categories: unix
title: "Error Packages Requests di Bench Frappe"
---

Jadi gini perintah `bench update` pada Frappe agak menakutkan selain update kode <em>apps</em> juga update `bench` dan paket python yang dibutuhkan. Masalahnya paket tersebut tidak sesuai dengan kode lama. Sehingga terjadi masalah. Salah satu masalahnya adalah paket `requests` dari python terupdate. Berhubung ini paket utama dan jika terjadi masalah seluruh sistem akan terimbas dampaknya. 

<img src="/img/erro_erpnext.png">

Setelah mencari di pusat data `Google` paket `requests` yang dibutuhkan `Frappe` adalah versi `2.6.0`. Sedangkan di sistem biasanya diatas versi tersebut. Kendalanya lagi jika hanya diturunkan tanpa dihapus paket lama masih tersimpan. Ekseskusi paket akan memakai yang lama. Untuk itu harus <em>reinstall</em>. 


```
pip install --upgrade --force-reinstall `requests==2.6.0`
```

Sekian. 
