---
layout: post
title:  "Rust Telegram Bot"
author: hafid
categories: [ tutorial ]
image: assets/images/crab.jpg
---


Rust Telegram bot using Teloxide and running using systemd

Beberapa hari yang lalu sudah menyelesaikan bot untuk menyatat belanja sehari-hari. Platfrom yang dipilih telegram karena mudah.
Dan banyak yang sudah membuatnya jadi tinggal amati tiru modifikasi(maling). Nah karena kebetulan ingin belajar crab mental model jadinya dibuat menggunakan Rust.
Karena awalanya pakai konsep ATM maka bot nya pakai [teloxide](https://github.com/teloxide/teloxide) untuk mempermudah. Selanjutnya tinggal deploynya.

Sebagai tambahan karena penasaran dengan [nix](https://nixos.org/) akhirnya coba-coba untuk development menggunakan nix. Pertama mencoba https://github.com/nix-community/crate2nix
namun terkendala update version yang entah karena bodohnya saya tidak menemukan cara update rustc, sempat ganti cargo2nix namun mengalami kendala yang sama.
Akhirnya development menggunakan setup manual nemu dari tulisan [fasterthanlie](https://fasterthanli.me/series/building-a-rust-service-with-nix). 
Namun karena agak malas untuk deploy pakai fly.io akhirnya hanya digunakan sebagai development saja. 

Sebenarnya ingin juga mendeploy dengan nix namun deploy dulu dan running menggunakan [systemd](https://id.wikipedia.org/wiki/Systemd) karena kalau ada yang mudah kenapa ambil yang sulit. 
Nah karena belum ada CI/CD nya selanjutnya mungkin mau buat CI/CD deploy dengan github action biar gaul dikit. 

## Step deploy

Pertama build dulu rust apps nya. Dengan perintah berikut otomatis akan terbuat folder targets dan binary akan tersimpan pada folder targets/bin
```Bash
$ cargo build --relase
```

Kedua membuat config untuk running systemd nya. Buka dulu dengan editor yang vim banget. 

```Bash
$ sudo vim /etc/systemd/system/belanjaku.service
```

Lalu menambahkan hal berikut 

```config
[Unit]
Description=Aplikasi pencatat belanja harian
After=network.target
StartLimitIntervalSec=10 # start service setelah 10 detik os berjalan
[Service]
Type=simple # type ini ada banyak bisa dicari disini https://wiki.archlinux.org/title/Systemd
Restart=always
Environment="TELOXIDE_TOKEN=" # masing enviroment ditulis terpisah baris.
Environment="REDIS_URL="
Environment="SUPABASE_URL="
User=mnjhjs
ExecStart=/var/www/belanjaku/target/release/belanjaku # folder binary hasil dari build
~
```
Nah simpan dan keluar dari lophole VIM dengan cara pastikan dulu pada mode NORMAL dengan cara menekan tombol escape kemudian tekan Shift + : lalu ketika wqa! dan enter. Voila selamat sudah keluar dari VIM.

Sebelum menjalankan jangan lupa reload dulu daemon-reload untuk menyambungkan file service tadi dengan daemon.
```Bash
sudo systemctl daemon-reload
```

Lalu start service dengan perintah berikut. 
```Bash
sudo systemctl start belanjaku
```

Nah error terjadi, untuk mencari tahu apa yang error bisa pakai perintah berikut. 

```Bash
sudo journalctl -u belanjaku
```

Biar langsung jalan ketika vm tiba-tiba restart gunakan enable.
```Bash
sudo systemctl enable belanjaku
```

Apabila ingin menghentikan bisa menggunakan perintah berikut: 

```Bash
sudo systemctl stop belanjaku
```

Begitulah kira-kira jalan dengan systemd mudah bukan. Nah selanjutnya buat UI dashboard nya pakai reactjs dan nix tentunya.
Kalau mau lihat kodenya bisa kesini saja [belanjaku](https://github.com/hapidznur/belanjaku)

