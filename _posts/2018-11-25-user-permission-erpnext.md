---
layout: post
title: "Studi kasus user permission ERPNext"
author: "hapidznur"
category: Journal
tags: [ERPNext,Case Study]
image: spools.jpg
---

Studi Kasus:

Budi adalah kasir pada toko yang menggunakan ERPNext. Sebelumnya Budi bisa melihat Harga beli maupun harga jual pada toko dia bekerja. Pemilik toko ingin Budi tidak bisa melihat harga beli dari barang yang dijual. Di toko tersebut punya 2 Price List Standard Selling dan Standard Buying. Pemilik ingin Budi hanya bisa melihat Standard Selling pada Item Price. 

Dari permasalahan tersebut tidak bisa hanya menggunakan Role Permission Manager. Namun harus menggunakan User Permission juga. 

Pertama atur `Role Permission` `kasir` agar bisa membaca `Item Price` jika belum.

[Set Item Price Role Permission](/assets/erpnext_set_role_permission.png)

Kemudian Set Apply Permission pada `Item Price` `Role Permission` kasir pada langkah sebelumnya. 

[Set Apply Permission](/assets/erpnext_apply_permission.png)

Klik `Select Document Types` dan Pilih `If Price List is permitted` Kemudian Klik `Set`

[Set Permitted Document](/assets/erpnext_permitted_document.png)

Beralih ke `User Permissions`. Pilih user budi kemudian pilih `Doctype` `Price List` selanjutnya `Standard Selling`.

[Set User Permission](/assets/erpnext_user_permission.png)

Lihatnya di `Item Price`. 

[Item Price](/assets/erpnext_item_price.png)

Kasus diatas bisa diaplikasikan pada kasus lain yang hampir serupa. Hanya perlu diperhatikan `Doctype` yang terhubung.

