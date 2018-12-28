---
title: "Docker for Dummy With Golang & MySQL"
date: 2018-12-13T13:38:33+07:00
draft: false
tags : [Golang,MySql,Data,Docker]
categories : [Programming,IT,Devops]
metaimage : "https://cdn-images-1.medium.com/max/800/1*CO20-3P183ZAqrsJlF7n_A.png"
---

![Docker_For_Dummy](https://lh3.googleusercontent.com/fvAGQxdVsOlGVvXkWyjgjQAwrOaoT1zrct_4FCj-9GD2QStUH3RVGWNGVh6EMIdcWZPHvsEluLTHlltDcVRdkh1eXNEJjZ3bq6YEuEc2McOrONkNsMpfAjB3VKTkZMiWsamFMKCXmjpPzeyUnX5vHkjiS5HwpZXjZZXMLMc-GsdXZjfABZbuXp_ZKO35GHZsadONLzFbBzgy3uMxS_P_reKFj-rqUvTf-_mrnwCklAz7a-kjDa7NskNceBd0AtliEZzZtNdEKIQFU_HT5jpGLRir8jQl6gclcyLG-dtGihyQbhzeIg7OmY8xyrO2LqnuRlozZbmT4fEPRL1uqTTV9G0ecO65NlGeL0LydjA-nzgmLJozXg12RwldyYfQjlIloMhgHR6mjZm2bGtHDipHqPbstTva24RBkURIIutoeUmYXCByO0wXMID4ja-QP1EVgVczjYTw5kKYmHb70kTQyw-gUV6pECjvYGWl0oFtUshkO2_mmLtYhfUj_-bLyR63OutRAhHUoP9HPc3diWCn9S0W13OjZsTydwGWHj20Tyev3W5xZkGYPBZAF8I-u3uyKZpK7Oq-hvJj4rUMe5p4lCjnoaMmHd-TaTV_QaVBLfcrOpFA9by-9eOwCf4w75mdVNHt2fDe0j-jE0z-UjMeCmwo=w533-h193-no?w=200)

# Say Hi...
Halo teman-teman, lama belom nulis karena sedang ada beberapa kesibukan dan harus belajar yang bener sebelum nulis hehe.
Pada kesempatan ini, saya akan membagikan pengalaman saya mengenai bagaiamana mengoperasikan docker secara sederhana melalui contoh aplikasi 
Golang. Tentunya artikel ini bukan artikel yang ditujukan kepada _mastah-mastah_ yang sudah malang melintang di dunia **_devops_**. Saya menulis artikel ini
dalam perspektif supaya saya yang seorang **_developer_** sedikit tahu apa yang dilakukan teman-teman devops supaya ketika bekerjasama nantinya 
akan mendapatkan hasil yang efektif.

# Ngapain sih pake Docker?
Secara sederhana saya akan coba menjelaskan sebeneranya apa tujuan menggunakan docker di dalam lingkungan software development. 
Docker dalam perspective saya adalah tools untuk menyamakan _environtment_ antara komputer lokal, server development , dan server production.
Sehingga Docker ini dianalogikan sebagai tools untuk membuat sebuah _container-container_ yang berisi binary aplikasi yang siap dijalankan, 
_software developement kit_ , _nginx_, _mysql_, dsb yang saling ketergantungan dan versi nya harus sesuai untuk menjalankan aplikasi.
Keuntungan lainnya seperti aplikasi yang terisolasi dan kemudahan dalam melakukan _scalability_ , tapi kita tidak akan panjang lebar di teori.
Mari kita PRAKTEK!!! **#HiyaHiyaHiya**

# Bikin Golang & MySQLnya dulu Braderr...
Asumsinya, temen-temen sudah menginstall docker , golang, mysql di komputer masing-masing **#HiyaHiyaHiya**. Sebelum mulai, silahkan buat database dan data nya dulu dengan MySQL :
```mysql

CREATE DATABASE dockeraja;

CREATE TABLE `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(100) NOT NULL,
  `is_active` int(11) DEFAULT NULL COMMENT 
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=18 DEFAULT CHARSET=latin1;

// lalu silahkan Insert dengan data kalian sendiri suka suka ... #HiyaHiyaHiya

```

Oiya jangan lupa kita harus memastikan supaya aplikasi docker dapat mengakses host mysql yakni dengan cara :
### Dapatkan data ip address kita yang bisa diakses dengan ifconfig (linux/macOS) xixixixi :D
```text
~indraoctama-terminal>ifconfig
en6: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=3<RXCSUM,TXCSUM>
	ether 00:50:b6:1c:be:b6 
	inet6 fe80::107c:152c:8376:7b3%en6 prefixlen 64 secured scopeid 0x5 
	inet 192.168.10.191 netmask 0xffffff00 broadcast 192.168.10.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect (100baseTX <full-duplex,flow-control>)
	status: active
```
Setelah melakukan _command_ di atas, kita mendapatkan ip address yang bisa diakses adalah **192.168.10.191**. Sehingga kita nanti akan melakukan set 
DB_HOST= **192.168.10.191** dan DB_PORT=**3306** (default). Untuk user dan password mysql kalian bikin sesuai keikhlasan masing2 :D. 
Jika teman-teman mengalami kendala tidak bisa mengakses dengan kondisi host di atas bisa mencoba artikel ini :
[GRANT mysql user to remote IP](https://stackoverflow.com/questions/6239131/how-to-grant-remote-access-permissions-to-mysql-server-for-user)

### Lanjut ke Golang
Kita akan membuat struktur folder aplikasi seperti gambar berikut ini :
![Struktur_Folder](https://cdn-images-1.medium.com/max/1600/1*w7vmvnTXSTkKOkTmwggdKQ.png)
#### main.go
{{< gist indraoct a8225656a18f2e47b549c1099a5d2a9d >}}

#### Dockerfile
```text
FROM golang:1.11.1

MAINTAINER Indra Octama omyank2007i@gmail.com
ADD . /go/src/dockerinaja

ARG appname=e-document-api
ARG http_port=1323

RUN go get -d github.com/go-sql-driver/mysql
RUN go get -d github.com/labstack/echo
RUN go install dockerinaja

ENTRYPOINT /go/bin/dockerinaja

ENV PORT $http_port
ENV DB_HOST 192.168.10.191
ENV DB_PORT 3306
ENV DB_USER userdocker
ENV DB_PASS passdocker

EXPOSE $http_port

RUN mkdir -p /go/src/dockerinaja
COPY . /go/src/dockerinaja
WORKDIR /go/src/dockerinaja

CMD go run main.go

```

# Dockerizing ... #HiyaHiyaHiya
Setelah selesai ngodingnya, maka kita bisa cobain _command_ berikut (tentunya masih dalam satu folder "dockerinaja" diatas) :
### Build image apps
```text
docker build -t dockerinaja .
```
Command di atas akan mengenerate docker image untuk keperluan menjalankan aplikasi.

### Check Images
Kita dapat mengecheck apakah image sudah terbentuk? Yakni dengan _command_ berikut :
```text
docker images
```
Lalu penampakan nya kurang lebih seperti ini :
```text
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
dockerinaja         latest              78de7207aa5a        3 minutes ago       952MB
golang              1.11.1              45e48f60e268        2 minutes ago       777MB
```

### Run image aplikasinya 
Lalu kita dapat menjalankan aplikasinya sebagai berikut :
```text
docker run -p 1323:1323 -t dockerinaja
```
untuk check apakah sudah running atau belum dengan cara berikut :
```text
docker ps
```
Lalu akan terjadi penampakan sebagai berikut :
```text
CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS         PORTS                   NAMES
9e29660a6901   dockerinaja  "/bin/sh -c /go/bin/…"   5 seconds ago    Up 5 seconds   0.0.0.0:1323->1323/tcp  upbeat_hamilton
```

Kita telah berhasil menjalankan aplikasi golang dengan docker, mari kita coba dengan insomnia :

![InsomniaAPI](https://cdn-images-1.medium.com/max/1600/1*-NnTPmMZVp-xc7pp_N6lDg.png)
Oke sekian hasil percobaan yang saya lakukan, semoga teman-teman dapat melakukan percobaan sesuai dengan hasil yang diharapkan, kalo belom coba banyak berdoa **#HiyaHiyaHiya**

[Source Code](https://github.com/indraoct/go-docker)