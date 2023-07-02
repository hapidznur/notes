---
layout: post
title: "Anatomi Git Commit"
author: "hapidznur"
categories: journal
tags: [Tulisan,Git,Tree,TrunkBasedDevelopement]
image: spools.jpg
---

Ide Tulisan ini setelah ikut kelas dari Pak @ldoreno ngadain kelas Git Workshop dan Trunk Based Development.

Kenapa dengan git commit? ya ndak papa. Git commit ya gitu-gitu aja :D. Alasan lain untuk ikut kelas adalah pengen tahu Trunk Based Development. Karena sebenarnya lagi nyari metode Development yang lebih enak dari Git Flownya github atau Git flow nya atlasian. Nah dari tahun kemarin kok kayaknya TBD ini menarik. Jadilah ikut kelas ini. 

Untuk kelas syaratnya harus paham git basic dulu. Kenapa? mau bongkar motor tapi gak bisa naik motor mau test drive nya gimana. Jadi gitu. Tulisan ini jauh dari kata sempurna. Jadi kritik dan saran bisa lewat twitter @hapidznur karena masih malas buka comment di blog ini. 

## Git Commit Anatomy

Commit dalam git adalah [first class citizen](https://en.wikipedia.org/wiki/First-class_citizen) dari git. Karena dari commit lah kita mengetahui perjalanan waktu dari sebuah kode yang dibuat. 

Misalnya lagi ngerjain sesuatu. Kemudian sudah sampai fase:
```bash
vim kerjakeras.md # nulis kode telah selesai dan sampai perintah :wq! 
git add kerjakeras.md
git commit -m "tambah file baru"
```

Maka otomatis git membuat commit object yang dinamai dengan hash dengan sha1. Contoh hasil hash SHA1:
```
$ echo -n "thoughtram" | openssl sha1
a9eb85ea214a6cfa6882f4be041d5cce7bee3e45
```
Pasti sering menjumpai saat git log 40 karakter diatas. Kenapa sih ada hash pada git. Menurut [thoughtram][1] kenapa git mengunakan sha1 yang terdiri dari 40 karakter dan tidak menggunakan 5 karakter nama sederhana. Karena: [Git has integrity][2]. Dengan mengunakan sha1 hasil hash dihasilkan berbeda satu sama lain. Pada sha1 hash terbentuk akan berbeda jika terdapat perbedaan meskipun hanya 1 karakter bedanya. Sehingga dapat dipastikan file yang disimpan tahun lalu adalah file yang sama diakses hari ini. 

Dalam sebuah Git Commit input untuk sha1 adalah:
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

Dalam git semuanya adalah object. Nah ada 4 tipe object dalam git:
1. Commit objects
2. Annotated Git Object
3. Blob
4. Tree Objects

Commit object seperti contoh diatas adalah hasil hash dari commit yang telah dibuat. Blob adalah file compressed dari sebuah file yang disimpan. Tree objects adalah sebuah Tree dari struktur file yang dicommit. Annotated Git object adalah pertanda commit yang mengunakan tag. Pada tulisan ini akan lebih banyak membahas Commit Objects, blob, dan Tree Objects. 

## Hashes over hashes
Kenapas sih hash over hashes. Setiap object dalam git adalah hash dari hash file. Karena setiap file dari Git akan di hash dan di hash lagi oleh object lain diatasnya. Misalnya terdapat susunan folder seperti berikut
```
.
|-- .git
|-- assets
|   |-- logo.png
|   |-- style.css
|-- app.js
```

Dari susunan diatas dapat digambarkan seperti berikut:
![Gambar Tree Objects dari git](/assets/img/git_tree.png)
Sumber:[Presentasi](https://docs.google.com/presentation/d/1d_ZoCUZdABJJCH6wLeIeyw0ND2CEmRLQmc-XdzwM-KQ/edit#slide=id.g34cd39258f_0_28)

Gambar diatas adalah sebuah Tree objects dari git. Pada gambar tersebut terdapat sebuah file `logo.png` dan sebuah hash untuk file tersebut `aa1b2fb696a831...` yang mana hash merujuk pada sebuah file blob dari `logo.png`. Kemudian terdapat file `style.css` dengan 
hash file `4c511f16ef2644854d....` yang merujuk pada pada file blob dari `style.css`. Kedua file `logo.png` dan `style.css` terdapat pada direktori assets. 

Direktori asset mempunyai hash `7cf2a17f33456...` yang merupakan representasi dari Tree object. Tree object adalah sebuah diksionari. Tree objects didalamnya terdapat tree object dari file yang terdapat hash yang merujuk pada sebuah blob. 

Nah direktori asset dan `app.js` pada satu direktori paling atas. Asset mempunyai hash `7cf2a17f33456...` yang merujuk pada sebuah tree object dan `app.js` mempunyai hash `29bfcf9fa582433...` yang merujuk pada blob. Kemudian direktori root ini di hash lagi menjadi sebuah tree object dengan hash `9c435a86e664be00d...` yang menjadi root dari tree object. 

Oke. Pusing? Nanti tak benerin dulu. Sementara ini dulu. Diulangi sekali. Sebuah hash `9c435a86e664be00d...` adalah sebuah tree object yang didalamnya merujuk pada sebuah tree object atau diksionari yang merujuk pada tree object lain dan sebuah file blob. Tree object yang kedua merujuk pada sebuah .... Nah dan begitu seterusnya.

## Commit Object

Nah input dari SHA1 sudah disebutkan diatas. Berikut contoh untuk sha1 dari sebuah commit:
```
sha1(
  commit message 
  => "initial commit"

  commiter
  => Christoph Burgdorf <christoph.burgdorf@gmail.com>

  commit date
  => Sat Nov 8 10:56:57 2014 +0100

  Author
  => Christoph Burgdorf <christoph.burgdorf@gmail.com>

  author date
  => Sat Nov 8 10:56:57 2014 +0100

  Tree
  => 9c435a86e664be00db0d973e981425e4a3ef3f8d
  
  Parent
  => [9c435a86e664be00db0d973e981425e4a3ef3f8d, jk311nn3334k3kj1yuo31fg12pb35mlk99o2o1]
))
```
Sumber: [Presentasi](https://docs.google.com/presentation/d/1d_ZoCUZdABJJCH6wLeIeyw0ND2CEmRLQmc-XdzwM-KQ/edit#slide=id.g34cd39258f_0_46)

Nah dari hasil commit diatas begini kira-kira gambaran Tree yang dibuat. Seperti penjelasaan diatas Parent akan terisi apabila object tersebut merupakan child. 
![Gambar Tree Objects dari git](/assets/img/git_tree.png)
Sumber: [Presentasi](https://speakerdeck.com/schacon/a-tale-of-three-trees?slide=13)


Nah begitu penjelasan git commit anatomy. 



Untuk bikin aplikasi seperti membuat lukisan. Buat sketsa jelek baru buat yang bagus.

Pustaka:
1. https://blog.thoughtram.io/git/2014/11/18/the-anatomy-of-a-git-commit.html#whats-up-with-those-long-revision-names
2. http://shafiul.github.io/gitbook/1_the_git_object_model.html
3. https://blog.thoughtram.io/git/rebase-book/2015/02/10/understanding-branches-in-git.html
4. https://en.wikipedia.org/wiki/First-class_citizen
5. https://drive.google.com/drive/folders/1HIjrCk4Ss2qcXXvY23Vf8OjV535cd3Xe?usp=drive_open
6. https://speakerdeck.com/schacon/a-tale-of-three-trees?slide=84
7. https://docs.google.com/presentation/d/1d_ZoCUZdABJJCH6wLeIeyw0ND2CEmRLQmc-XdzwM-KQ/edit#slide=id.g34d24d4cfd_0_0
