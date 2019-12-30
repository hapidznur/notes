---
author: hapidznur
categories:
  - journal
image: spools.jpg
tags:
  - Tulisan
  - Git
  - Tree
  - TrunkBasedDevelopement
title: Git Workshop
url: /2018/03/10/git-trunk-based-development/
---



## Git Commit

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
