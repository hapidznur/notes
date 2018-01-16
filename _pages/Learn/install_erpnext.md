---
layout:page
title: Install Frappe and ERPNext manual
permalink: /learn/install_erpnext/
---

## Install Frappe and ERPNext manual
[ERPNext][1] ini salah satu software erp(enterprise resource planning) yang opensource. Meskipun penggunanya belum sebanyak odoo, openbravo, dynamic ataupun SAP tapi cukup lengkap sebenarnya. Meskipun kalau boleh protes terlalu banyak magic didalamnya. Untuk ERPNext sendiri hanya bagian module dari [Frappe][2]. Frappe semacam framework layaknya Django. Meski magic ada dimana-mana tapi framework ini cukup bagus secara desain dan kemudahan untuk user memahami.
Mungkin baru beberapa bulan ini belajar ini. Namun masih banyak yang belum saya pahami dari framework ini. Saking terlalu banyaknya magic yang ada. 
Dengan campuran dari python dan javascript framework ini cukup powerfull sebenarnya. Sederhana dan fleksibel. 

Sebelum install frappe butuh requirement yang dibutuhkan:
1. Python 2.7
2. MariaDB 10+
3. Nginx (for production)
4. Nodejs
5. Redis
6. cron (crontab is required)
7. wkhtmltopdf with patched Qt (for pdf generation)

Sebelum meginstall erpnext tambahkan script berikut pada akhir `/etc/my.cnf/`.
```
[mysqld]
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```
Untuk menginstalnya diperlukan bench. Bench ini semacam package managernya. Install bench dulu. Oh ya install sebagai non-root user saja. Bahaya. 
```
$ git clone https://github.com/frappe/bench bench-repo
$ pip install --user -e bench-repo
```
Setelah diinstall akan ada folder bench-repo __jangan dihapus__

Oke setelah bench terinstall selanjutanya install ERPNext. Perintah init berfungsi membuat folder yang berisi satu framework. Didalamnya terdapat perintah, config dan segala macam keperluan ERPNext. Setup ini akan otomatis update dalam jangka waktu tertentu. 
```
bench init erp && cd erp
```
Kemudian membuat website. Tentu saja dibutuhkan karena frappe ini ada website framwork. Untuk menjalankannya dibutuh minimal 1 site. 
```
$  bench new-site site1.local
```
Karena ERPNext adalah module yang terpisah maka perlu diinstall lebih dulu.
```
$  bench get-app erpnext https://github.com/frappe/erpnext
```
Setelah terinstall perlu disambungkan dengan website yang telah dibuat tadi
```
$  bench --site site1.local install-app erpnext
```
Untuk menjalankannya. Perhatikan untuk selalu pada folder `erp` untuk menjalankan perintah bench ini. 
```
$ bench start
```

ERPNext sudah bisa digunakan.


















