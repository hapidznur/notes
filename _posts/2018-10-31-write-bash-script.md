---
layout: post
title: "Write Bash Script"
author: "hapidznur"
category: ngangsukaweruh 
tags: [documentation,bash,script,linux]
image: spools.jpg
---

Jadi untuk ceritanya butuh menjalankan _script_ otomatis _crawler_ di sistem operasi Linux. Setelah berguru pada mbah segala ilmu pengetahuan saat ini a.k.a google. Akhirnya menemukan _script_ yang pas dari guru _programming_ stackoverflow.com[[1]]. Sedikit modifikasi saja dari saya. 

```bash
check_process(){
        # check the args
        if [ "$1" = "" ];
        then
                return 0
        fi
        #PROCESS_NUM => get the process number regarding the given thread name
        PROCESS_NUM=$(ps -ef | grep "$1" | grep -v "grep" | wc -l)
        # for degbuging...
        $PROCESS_NUM
        if [ $PROCESS_NUM -eq 1 ];
        then
                return 1
        else
                return 0
        fi
}

# Check whether the instance of thread exists:
while [ 1 ] ; do
        echo 'begin checking...'
        check_process "python test_demo.py" # the thread name
        CHECK_RET=$?
        if [ $CHECK_RET -eq 0 ]; # none exist
        then
                # do something...
        fi
        sleep 60
done
```

_Script_ tersebut kemudian disimpan dengan *nama_file.sh*. Untuk menjalankannya tinggal `sh nama_file.sh`. Jika ingin menjalan pada proses belakang layar tambah simbol `&` pada akhir perintah. 
```
$ sh nama_file.sh &`
$ disown %h 1 # tetap jalan setelah terminal ditutup
```

[1]: https://stackoverflow.com/questions/7708715/check-if-program-is-running-with-bash-shell-script

