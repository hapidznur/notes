---
layout: post
title:  "Funtify: Read Config"
author: hafid
categories: [ funtify ]
image: assets/images/funtify.png
---

Variabel enviroment sengaja saya pisah agar lebih memudahkan dan bisa menganut prinsip 12 app factor principles. Nah ini terdapat pada file `config.go`. 

Untuk membaca file enviroment gak perlu capek-capek bikin sendiri ya walaupun bisa tapi membuang waktu. Untuk membaca enviromentnya menggunakan [Viper][1].
Jangan lupa import viper. Kalau pakai visual studio dan terinstall extensi untuk Go otomatis ter-import apabila sudah ada di local dan terhapus apabila tidak terpakai.  
```go
import (
	"log"

	"github.com/spf13/viper"
)
```

Nah ini untuk membaca file konfigurasinya. 
```go
func readEnvVariable(key string) string {

    // Nama config yang akan digunakan
    viper.SetConfigName("env")
    // Path folder config 
	viper.AddConfigPath(".")
	// membaca seluruh config yang ada pada file tersebut
	err := viper.ReadInConfig()

	if err != nil {
		log.Fatalf("Error while reading config file %s", err)
	}

    // Masukkan config tersebut pada array
	value, ok := viper.Get(key).(string)

	if !ok {
		log.Fatalf("Invalid type assertion")
	}

	return value
}
```

Pada projek ini menggunakan file dengan extensi `yaml`. Viper tidak terbatas pada `yaml` saja namun bisa extensi lain seperti JSON,txt,maupun `.env`. Jadi cukup mudah untuk menggunakan viper. 

Terima kasih sudah membaca.

[1]: https://www.github.com/spf13/viper