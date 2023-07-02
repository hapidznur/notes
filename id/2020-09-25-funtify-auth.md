---
layout: post
title:  "Funtify: Auth"
author: hafid
categories: [ funtify ]
image: assets/images/funtify.png
---


Spotify menggunakan OAuth2 untuk verifikasinya. Secara sederhana seperti gambar dibawah. Gambar diambil dari dokumentasi [spotify][1]. 
![Intro Flow]({{site.url}}/assets/images/auth_spotify_intro.png)

Secara alur otorisasi user harus mengizinkan aplikasi untuk mengakses akunnya melalui API. Nah kemudian setelah mendapat izin baru bisa mengakses dengan proses pengecekan melalui token yang dikirim oleh aplikasi ke Spotify API. 

Ada beberapa cara untuk otorisasi namun disini akan menggunakan kode. Aplikasi akan mengirimkan client_id dan client_secret dan akan menerima access_token yang akan digunakan untuk memverifikasi akses ke API Spotify. Untuk lebih jelasnya bisa dilihat pada gambar dibawah. Gambar flow authnya ada [disini][1]
![Authorization Code Flow](https://developer.spotify.com/assets/AuthG_AuthoriztionCode.png)

Urutan otorasasi menurut gambar tersebut:
1. Membuat url yang mengarahkan pada halaman verifikasi Spotify. 
2. Mengirim url untuk mendapatkan access_token. 
3. Nah setelah itu akan mendapatkan access token yang akan digunakan untuk melakukan requests ke WEB API.
4. Melakukan request access_token apabila telah kadaluarsa.

Proses OAuth2 menggunakan library OAuth2 dari google agar mempermudah dan tentunya mempercepat develop. Namun untuk memahami prosesnya kayaknya memang perlu buat sendiri. Untuk proses 1 dan 2 dibantu library proses. Proses 3 dan 4 akan sepenuh ditangani library, lebih enak to. 

Untuk koding disini saya copy paste dari  [zmb3/spotify][2]. Dengan sedikit modifikasi karena lebih banyak reverse untuk mempelajari flownya. 

##### Step 1
Membuat url untuk mengarahkan pada halaman verifikasi ini dilakukan manual oleh user. Karena ini hanya backend, tidak fokus pada frontend. URL tersebut berisikan client_id, response_type, redirect_url, state, dan scope.
```go
type Authenticator struct {
	config  *oauth2.Config
	context context.Context
}

func Authorize(redirectURL string) Authenticator {
	cfg := &oauth2.Config{
		ClientID:     readEnvVariable("spotify_id"),
		ClientSecret: readEnvVariable("spotify_secret"),
		RedirectURL:  redirectURL,
		Scopes:       []string{"user-read-private"},
		Endpoint: oauth2.Endpoint{
			AuthURL:  "https://accounts.spotify.com/authorize",
			TokenURL: "https://accounts.spotify.com/api/token",
		},
	}
    
	tr := &http.Transport{
            TLSNextProto: map[string]func(authority string,
            c *tls.Conn) http.RoundTripper{},
    }
    
    ctx := context.WithValue(context.Background(), oauth2.HTTPClient, 
            &http.Client{Transport: tr})
            
	return Authenticator{
		config:  cfg,
		context: ctx,
	}
}
```
Buat struct dulu untuk nyimpen variabel yang dibutuhkan agar supaya tidak kerepotan. Struct Authenticator isinya ada 2 yaitu `Config` dan `Context`. `Config` ini berisi konfigurasi yang dibutuhkan pada OAuth2 seperti client_id, client_secret, scope, redirect_url, dan endpoint. 

Untuk client_id dan client_secret saya simpan di env. Untuk mengakses variabel enviroment menggunakan viper yang sudah dibundel pada fungsi `readEnvVariable` yang selengkapnya bisa baca [disini][3]. Untuk scope dari Spotify API bisa dibaca di [dokumentasinya][4]. Untuk API ini hanya menggunakan scope `user-read-private`. 

`Context` dibutuhkan untuk mengirimkan value dari 1 proses ke proses selanjutnya. Tujuannya agar valuenya yang dibawa konsisten. Nah `context` sendiri diharuskan ketika ada request yang masuk maupun yang keluar. Fungsi `context` ada beberapa kayaknya perlu dibuat dokumentasinya biar lebih gampang dibaca kalau lupa. Nah Untuk `context` yang dipakai disini untuk mengirimkan value pada config serta hasil token yang didapat dari verifikasi.

Selanjutnya adalah untuk mengambil token dari Spofity API. 
```go
func (a Authenticator) Token(r *http.Request) (*oauth2.Token, error) {
	values := r.URL.Query()
	if e := values.Get("error"); e != "" {
		return nil, errors.New("spotify: auth failed - " + e)
	}
	code := values.Get("code")
	if code == "" {
		return nil, errors.New("spotify: didn't get access code")
	}

	return a.config.Exchange(a.context, code)
}
```
Nah diatas adalah sebuah method dari struct Authenticator. Authorize adalah sebuah fungsi. Yang membedakan adalah method punya receiverType sedangkan fungsi tidak punya. Nah bagian ini juga sekaligus melakukan handling untuk request access_token menggunakan client_id, client_secret, code, grant_type, redirect_uri. Nah bereslah sudah OAuth ndak perlu ngoding untuk requestnya ke spotify APInya dan misal access_token kadaluarsa yang library ini yang akan melakukan perbaruan. Mudah sekali bukan. 

Selanjutnya adalah bikin main prosesnya.

Di golang untuk bikin server http bisa menggunakan fungsi `ListenAndServe` dari library `http`. yang diikutin dengan port yang dituju dan file yang akan dijadikan halaman. Namun karena hanya ingin membuat serve untuk callback maka bisa diisi `nil`.
```go
http.ListenAndServe(":3000", nil)
```

Sebelum itu bikin fungsi untuk mengnangani callback dari API Spotify untuk nerima kode dan melakukan exchange ke Spotify lagi untuk mendapatkan token.
```go
func checkAuth(w http.ResponseWriter, r *http.Request) {
	token, err := auth.Token(r)
	if err != nil {
		http.Error(w, "Couldn't get token", http.StatusForbidden)
		fmt.Printf("%q", err)
		log.Fatal(err)
	}

	// use the token to get an authenticated client
	fmt.Printf("%q", token)
	fmt.Fprintf(w, "Login Completed!")
}
```
Tentunya fungsinya harus ada responseWriter dan request agar supaya bisa diterukan lagi ke context yang menangani permintaan token ke Spotify. Nah Sebenarnya mubazir sih ada log dan fmt tapi biar aja dulu. Setelah itu kalau pengen tahu hasilnya token seperti apa bisa dijalankan. 

Selanjutnya buat servernya dulu. Berikut bikin script servernya.
```go
var (
	redirectURL = readEnvVariable("redirect_url")
	auth        = Authorize(redirectURL, ScopeUserReadPrivate)
	state       = "superstring"
)

func main() {
	http.HandleFunc("/callback/", checkAuth)
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		log.Println("Got request for:", r.URL.String())
	})
	url := auth.config.AuthCodeURL(state)
	fmt.Println("Please log in to Spotify by visiting the following page in your browser:", url)

	if err := http.ListenAndServe(":3030", nil); err != nil {
		log.Fatalf("could not listen on port 5000 %v", err)
	}
}
```
Penggunaan `if` disitu untuk melakukan pengecekan apakah port tersebut digunakan atau tidak. Selain itu biar servernya terus jalan sampai dihentikan secara paksa atau portnya digunakan. Sebenarnya lebih enak pakai `channel` namun dokumentasi berikutnya aja. Biar lebih paham saya. 

Fungsi `HandleFunc` juga sebagai routing pada golang. sehingga itu adalah fungsi untuk mengnangani request pada setiap url-nya. 

Jangan lupa untuk mengeceknya harus dibuild dulu dengan perintah `go build` dikuti dengan seluruh file yang di-import di fungsi main. Hasilnya adalah sebagai berikut. 
![Login]({{site.url}}/assets/images/hasil_login.png)

Jangan lupa copy paste kan pada browser dan verifikasi di halaman Spotify dan otomatis akan menuju callback dan muncul halaman berikut.
![Berhasil]({{site.url}}/assets/images/berhasil_login.png)

Nah sudah berhasil. Demikian. 

[1]: https://developer.spotify.com/documentation/general/guides/authorization-guide/#authorization-code-flow
[2]: https://github.com/zmb3/spotify/
[3]: {{site.url}}/funtify-read-config/
[4]: https://developer.spotify.com/documentation/general/guides/scopes/