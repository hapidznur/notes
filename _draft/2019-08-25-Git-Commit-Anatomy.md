---
layout: post
title: "Anatomi Git Commit"
author: "hapidznur"
categories: journal
tags: [Tulisan,Git,Tree,TrunkBasedDevelopement]
image: spools.jpg
---



Tulisan ini seri kedua dari tulisan Git anatomy. 
Ide Tuisan ini setelah ikut kelas dari Pak @ldoreno ngadain kelas Git Workshop dan Trunk Based Development.

Kenapa dengan git commit? ya ndak papa. Git commit ya gitu-gitu aja :D. Alasan lain untuk ikut kelas adalah Trunk Based Development. Ceritanya lagi nyari metode Development yang lebih enak dari Git Flownya github atau Git flow nya atlasian. Nah dari tahun kemarin kok kayaknya TBD ini menarik. Jadilah ikut kelas ini. 

Untuk kelas syaratnya harus paham git basic dulu. Kenapa? ya mau bongkar motor tapi gak bisa naik motor ya mau test drive nya gimana. Jadi gitu. Tulisan ini jauh dari kata sempurna. Jadi kritik dan saran bisa lewat twitter @hapidznur karena masih malas buka comment di blog ini. 

## Git Commit Anatomy

Commit dalam git adalah [first class citizen](https://en.wikipedia.org/wiki/First-class_citizen) dari git. Karena dari commit lah kita mengetahui perjalanan waktu dari sebuah kode yang dibuat. 

Misalnya lagi ngerjain sesuatu. Kemudian sudah sampai fase:
```bash
vim kerjakeras.md # nulis kode telah selesai dan sampai perintah :wq! 
git add kerjakeras.md
git commit -m "tambah file baru"
```

Maka otomatis git akan lakukan hash dengan sha1. Contoh hasil hash SHA1
```
$ echo -n "thoughtram" | openssl sha1
a9eb85ea214a6cfa6882f4be041d5cce7bee3e45
```
Input untuk SHA1 adalah
1. Metadata Commit
    - Commit Message 
        => Pesan commit
           - Feature Material Issue Release
    - Commit Name
        => Identitas yang melakukan commit
           - Nisana Dilmukamadev 
    - Commit Date
        => Waktu dan tanggal commit
           - Sat Nov 8 10:56:57 2014 +0100
    - Author File
        => Penulis file
           - Hafid Hidayatullah
    - Authoring Date
        => Waktu dan tanggal file dibuat
           - Sat Nov 8 10:56:57 2014 +0100
2. Hash of Tree Working Directory
    - Tree of Working Directory
        => Hash dari Tree Directory yang disimpan
           - h2hddfsf92k12n121nh2k2k1mlk2jl213k
    - Parent Commit
        => Parent commit dari commit tersebut. misalnya 
           - [kj1k21312,12jjjlkh31]

Sebelum lebih lanjut dalam git terdapat 4 object:
1. Commit objects
2. Annotated Git Object
3. Blob
4. Tree Objects

Pada tulisan ini akan lebih banyak membahas Commit Objects, blob, dan Tree Objects.

Misalnya terdapat susunan folder seperti berikut
```
.
|-- .git
|-- assets
|   |-- logo.png
|   |-- style.css
|-- app.js
```
Dari susunan diatas dapat digambarkan seperti berikut:
![Gambar Tree Objects dari git](/assets/git_tree.png)

Kenapa commit? karena commit adalah dasar dari git. Jadi git dibuat untuk mengakomodasi commit. Kalau menurut le[@leornado].
```bash
$ echo -n "thoughram" | openssl sha1
8b564c0f9f8d608cb954993e565e380ea7e89b07
```

Commit ID selalu dibentuk dari hash sha1. Kenapa sih sha1. karena git butuh konsistensi dan integritas. 
Apasih sha1? SHA1 adalah fungsi hash. 
nah sha1 ini dibentuk dari apa sih?i
- committer
- commit date
- author
- date 
- Tree informasi dari directory dan apa yang dirubah dari commit tersebut dalam bentuk hash sha1
- Parent ID commit-commit dari sebelumnya

Object dari Git adalah:
1. Commit Object
2. annoted tag objects
3. blobs
4. Tree Object

Salah satu sha adalah Hash tree object directory. Contoh pada gambar git. 

Fungsi parent adalah sebagai integritas dari sebuah perubahan. Karena parent juga sebagai input dari perubahan. Sehingga ketika perubahan dilakukan diketahui informasi dari perubahan tersebut. 

Setiap commit yang dibuat maka akan membuat hash dengan sha1, tree object, membuat 1 commit object.
Misal punya file nama.txt
nama.txt = Commit0 -> Tree0 -> Blob0
nama.txt = Commit1 -> Tree1 -> Blob1

Git = Blockchain
Git menggunakan struktur dari Merkle Tree

Branch dan HEAD adalah pointer
Branch = Merujuk pada commit
HEAD = merujuk branch atau commit

HEAD merupakan rujukan untuk posisi saat ini kita ada branch mana dan pada commit mana.

Untuk memindahkan HEAD dengan
```
git checkout branch/hash commit
```

BEFORE COMMIT CHECK BRANCH WORKING DIRECTORY FIRST

NEED REDRAW on Papan

TRUNK BASED DEVELOPMENT
Apasih? Buat commit yang lurus-lurus aja. Gak kayak gitflow yang punya banyak cabang.

c1
  \
  |
  |
  |

# HOW TO TRUNKBASEDDEVELOPMENT
1. Git checkout feature/A 
2. Working on Progress
3. git fetch origin
4. git rebase origin/master
```markdown
fungsi rebase adalah merubah parent dari branch, membuat object baru, dan mereplay commit object dari yang sudah dibuat pada branch sebelum utama.
```
5. git checkout master 
6. git merge origin/master
7. git merge feature/A

Kenapa sih REBASE
Pro:
1. Conflict hanya akan terjadi pada feature/A


## git merge squash
Untuk menggabungkan semua commit menjadi 1 commit.
```
git merge --squash feat/a
```

Untuk bikin aplikasi seperti lukisan. Buat sketsa jelek baru buat yang bagus.
Pustaka:
1: https://docs.google.com/presentation/d/1d_ZoCUZdABJJCH6wLeIeyw0ND2CEmRLQmc-XdzwM-KQ/edit#slide=id.g34d24d4cfd_0_0

