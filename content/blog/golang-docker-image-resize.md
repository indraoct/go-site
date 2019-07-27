---
title: "Golang Docker Image Resize"
date: 2019-07-27T17:51:33+07:00
draft: false
tags : [Golang,MySql,Data,Docker,Resize,Alpine,Scratch]
categories : [Programming,IT,Devops]
metaimage : "https://lh3.googleusercontent.com/X87duYDhvYYxPB4my89QdLDlhtDTHd6AGtk8NyaUgg1T-LP03ApAdNYlDelkz0vrxgu3elX39hBBinnv1s0DQjBh9yyK_isNreQ1qfrDPGdCdRMTrQhvAt9u73XinzP4dUFn9-6Wzpv4Wq1ot3GEA4HVCk73hOMhYSw4O7qkQX7Xv9gbYhY9cfqHgTWG4CsaSJ6BBfpFRFjsrGNAoLK2-vH2tzyWWByRDAGMsVukVGPD09Ypgu2-y7oIwCRphHbVKmZLmqfY23_NWTAiHzklRj67vYnzsPqrXUggqHaUbiPSvmDNpY-oI5zsQPlKJDeBR8o9Dj0H1xogcYd40ZEfPlkT-ifx1NiZmAVg3k8ghwvmpkCLIvgpowT2cyIiFFdCOmLOnKp5hobUxcwUIvXTZGF1ti2DUZQEFaOYc2CLTpfYrRvGOyG9Gl7nEoTAefprLY55TiYHimzE2qZ2gddI-vCNqaTIkS7FReS9lY84pJXwytZQjkLroHuswyAuudR8y-s7Lu6v6wLbBUiuRnEbaUqG2SRAGQHnNgcMjm19SZ18czqQ3d-mO01IVsYVXMns07Vzt-4PfuTVNl0CsrSblBKxAI4G6hhK_4BxAB11QcztGufP6MnkFA4o07e_fqeaJAD440mercSMdH0PYCt8Ae2G_wzeNMs=w1100-h619-no"
---

![Golang Docker Image Resize](https://lh3.googleusercontent.com/X87duYDhvYYxPB4my89QdLDlhtDTHd6AGtk8NyaUgg1T-LP03ApAdNYlDelkz0vrxgu3elX39hBBinnv1s0DQjBh9yyK_isNreQ1qfrDPGdCdRMTrQhvAt9u73XinzP4dUFn9-6Wzpv4Wq1ot3GEA4HVCk73hOMhYSw4O7qkQX7Xv9gbYhY9cfqHgTWG4CsaSJ6BBfpFRFjsrGNAoLK2-vH2tzyWWByRDAGMsVukVGPD09Ypgu2-y7oIwCRphHbVKmZLmqfY23_NWTAiHzklRj67vYnzsPqrXUggqHaUbiPSvmDNpY-oI5zsQPlKJDeBR8o9Dj0H1xogcYd40ZEfPlkT-ifx1NiZmAVg3k8ghwvmpkCLIvgpowT2cyIiFFdCOmLOnKp5hobUxcwUIvXTZGF1ti2DUZQEFaOYc2CLTpfYrRvGOyG9Gl7nEoTAefprLY55TiYHimzE2qZ2gddI-vCNqaTIkS7FReS9lY84pJXwytZQjkLroHuswyAuudR8y-s7Lu6v6wLbBUiuRnEbaUqG2SRAGQHnNgcMjm19SZ18czqQ3d-mO01IVsYVXMns07Vzt-4PfuTVNl0CsrSblBKxAI4G6hhK_4BxAB11QcztGufP6MnkFA4o07e_fqeaJAD440mercSMdH0PYCt8Ae2G_wzeNMs=w1100-h619-no)

Golang dan docker adalah paduan yang ciamik ketika kita membuat aplikasi microservice yang berjalan di server / cloud.
Mengingat kembali artikel ini [Docker For Dummy](https://indraoctama.com/docker-for-dummy-with-golang-mysql-20181213/).
Lalu muncul pertanyaan, mengapa file aplikasi yang hanya 8-an MB dikonversi ke docker menjadi 900-an MB?


```text
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
dockerinaja         latest              78de7207aa5a        3 minutes ago       952MB
golang              1.11.1              45e48f60e268        2 minutes ago       777MB
```

Tentu saja ini berpengaruh pada saat melakukan docker push, docker pull, dan menghabiskan banyak space di harddisk.
Lalu saya mulai cari artikel bagaimana caranya meresize docker images supaya dapat diresize seminimal mungkin. Saya menemukan
artikel yang pas dari mas Huseyin BABAL [Micro Docker Images for Go Applications](https://blog.kloia.com/micro-docker-images-for-go-applications-8a8701130c01).
Di sana saya menemukan 2 cara yakni menggunakan golang alpine dan build from scratch.

## Cara 1 : Golang Alpine

Docker Alpine images adalah images dengan dependensi minimal yang berarti kita tidak akan mendapat banyak tools di 
dalamnya ketika kita menjalankan aplikasi golang ini. Untuk kasus-kasus umum, biasanya kita tidak membutuhkan images alpine ini.
Berikut adalah code main.go saya :

![Source Dode](https://lh3.googleusercontent.com/fcB2PCuQJOStv0_dLTTcG7LE0PM_wQ3iOTOAmt3IKeNaPVcnRTY-ncmN_Krd3zy8BN_O4IdJXni49IJtgFtp9RQlKr1KvlBNHy4cPZeUI9TAG0ZNWMyxxjhg9lB_lB1l2BlXYSgxiQEtMmymy4RehiprAYM4Zsfd6qNE8FefbHq8HO8At3XqyUzs-7YTNkD20nmWX58kvt25885reD9ANkTp9EeV6N6lB-SNnILPMY7pOklUyBzZu7lO7ebNWGs2hDb7vHSY_L0TOG-ksTfENgKqeYOUvzC01zd9fz-rgPEyM98Th02hdabOxwOw5jCmk0BwLCX549qfdywP1rVWSiAB_3GJpnDghBtTZIOZK_KYunMy7GE8GvbMFegnL1gcGsl4sKHcqVbrWJdjFul0NwLiDN1YERgzPDcL_GMA3sfemK9G0NOhYWu4JrOiX_gZTDCbeE-kNhX-o3SU-QTZjgexVHGEEn4VjkuBDvYsoewcjXLEsegGMj3hI2C3EGuuCqtA_1aERLgK1AdVWGmxlV-LDAWTD-YOckgH5x92NsM8wMh3wrUPtff3ClajVsZFZGnItl3rSHm-qafUBw0oUGh-hBM8U8zssUjFLS_FixGDTzSnDrafsjhiTGy2xBBLRdqawvJ1bDAOkNeulXf_nbzjyPN_qPk=w1726-h1544-no)

Dan berikut adalah isi Dockerfile saya :

```$text
FROM golang:alpine

MAINTAINER Indra Octama omyank2007i@gmail.com

ADD . /go/src/dockerinaja

ARG appname=dockerinaja
ARG http_port=1323

RUN apk update && apk add git

RUN go get -d github.com/go-sql-driver/mysql
RUN go get -d github.com/labstack/echo
RUN go install dockerinaja

ENTRYPOINT /go/bin/dockerinaja

ENV PORT $http_port
ENV DB_HOST yourhost
ENV DB_PORT 3306
ENV DB_USER yourusername
ENV DB_PASS yourpassword

EXPOSE $http_port

RUN mkdir -p /go/src/dockerinaja
COPY . /go/src/dockerinaja
WORKDIR /go/src/dockerinaja

CMD go run main.go
```

Lalu mulai kita jalankan script berikut ini :

```$text
docker build -t dockerinaja .
```

Seteleh selesai maka kita bisa melihat hasil docker imagesnya :
```$text
docker images
```
![Hasil Alpine](https://lh3.googleusercontent.com/ZfKfqKu81ulHrcWU15G2wW4eojRNs8ukFDsjBol2UEtbRVZm9TqdThbyzDlqxuE93dBVby2LtvxF7V0ftQCWRLBeNEMLMAcbA7XoQKfAo4IF17jUM3M1LsY9J6aLDasKfQ79CE4_Po2QOBfz7wDvQmvLFWRNLhLW030eQkWAjPIOzxXD7T2sUu8Nl2_8myEDxp7KT3ZV9X3-DbreK4RlMaB-PbIinYfMss1XymVLHu3VcWXCHLw8H5T6LEPF9QPHDmML3bhQ7DTmtU7hcOna53FFEWh9cprSD-SUn24XRPEWAEe-_Z_R0C5dm1iWd_nWqBQc600D-OHV4rDQkhlGl8veDTBdsbBXqkiJsKdaoXJDTH8nV3bXkpYD1E3aUlb15jNzPc-W4XoL-Fpz_EnVCyJwTyRtDVBLb8_T9zQqJoz_GYK9ammvn4qutFWdS-dJOVrsIGvvC5z4XN533_6KEYW5rF-0-dAjmwtnytYIMMhMMAzRpFcvAglGY524jnk-KoW0rB6IkBarrdoC2Il3ib-tM3CCh9RN7zY0kBekGSuKIDggUb3AQiBxa58ZgfH5nVBTrGkA3X7HwptdXn5-KS4wkOxHFrZwcuvHN8pb3Iyo9XKj0pl7QgLuK-zIObhJmg6HrRitZqRp_3dyUCFyQKR59xIaQvU=w1310-h118-no)
Di sini kita mendapatkan bahwa size images yang tadinya 900-an MB menjadi 511MB , sehingga berkurang sekitar 400MB. Tapi 
menurut saya ini masih terlalu besar untuk ukuran aplikasi Go yang sesederhana itu.

## Cara 2 : Build From Scratch
Supaya images yang dihasilkan lebih kecil, menurut artikel tersebut adalah melakukannya dengan teknik build from scratch (bikin dari 0).
Hal tersebut mereduce semua file yang tidak digunakan oleh aplikasi golang yang kita build. Kita dapat mengakalinya dengan cara 
mem-build binary file golangnya terlebih dahulu yakni dengan cara seperti ini :

```$text
CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .
```

Di sini dengan CGO_ENABLED = 0 dikatakan bahwa menonaktifkan cgo dan membangun aplikasi golang secara statis yang 
berarti kita akan memiliki semua dependensi begitu kita menyalin biner ini ke images. -a adalah untuk membangun kembali 
seluruh paket untuk memastikan kita memiliki semua dependensi. Setelah eksekusi ini, kita akan memiliki biner di dalam 
folder proyek kita.
![File binary](https://lh3.googleusercontent.com/MBGVGjrgzR9FbwXjg-U1sebzrzN9DLlxrQdv3zJ25MERbW-bhA3cBzM7F1Nxkyxb8-FFBdAs-NrcFjMMwbI2aMMn6kPNEqLLJ_qQoJffGudpxUq-cK7xkwqcVJ0v5b7J5mEGfuP_8pTKON9xUp_aY4RJzCx4PKnl4F1r9kSouS8E3XFPVSmfkUdm8ndojgiZy1YJcHDuPuKofvOhQbI8r9Us5CMAoewoianJKTc7OQTV_cQras8d6zVMzzBPt4vcdnbFnU65btHowvn_9AUDoAGhKAoBwoceAw8805O60oQrdc7ZP1K1hXU-3xRrc_ue8HgZ89R5XrkA10G3OwUzzxwFUNjooo-la3o39SJS-0uWFwjTyq1CwMRHlfAGgWVv2Df5V2VJ4OEWi2tLDUcnc9_OajV2Ltoiix16p4VaVdgazIAtx6Mz9ix1vuzJ8zK7RvAZ3HCOfPeyhwYb1nsVoKZDf9Tzt4b26ceqcHW6FuXHG41eFFLjX2mFmvEgtjWzxQ7ALMkFCLzoXNyUazGI2T7DAJiV5wXzrehzY1FtRlXMAHKuRBMCoryOKVe4X3f6TJ_MBzBoyXtzVEKoyOaFvAsy55j-aOMhj3I4S8WmKaBnZ8N7Z2CSLFKX73UCYBgKUhpDs_QcndLwzAx9h1oBHX7_H-ub1A8=w970-h334-no)

Berikut ini adalah file Dockerfile nya 
```$text
FROM scratch

MAINTAINER Indra Octama omyank2007i@gmail.com

ADD main ./

ARG appname=dockerinaja
ARG http_port=1323

ENTRYPOINT ["/main"]

ENV PORT $http_port
ENV DB_HOST yourhost
ENV DB_PORT 3306
ENV DB_USER yourusername
ENV DB_PASS yourpassword

EXPOSE $http_port
```

Kita ulangi lagi script berikut :

```$text
docker build -t dockerinaja .
```
Berikut ini hasil build images golang dari scratch :
![Scratch](https://lh3.googleusercontent.com/IdolkhxSCaqowquOh2Qb8IBWFiEx7av99U2Pjd2x7W5ETR5gXxD6lbOBJvVD3ci-qW3XIHmRXsCfbzwbZb-DBGQnLdLnBo8RZOPKnbXwmJMaCCMnSpwN5wkSKpQIcL5YRTWzJ3Qq3y8skmAsLfQRx1vxIxf7gfDUd_Q43f1L2MDuuFijZhBFbf1PaqdEyE-aVMK3m7mFzQWR--UafZO2eK01vgQ-G6ncwmJcbmE06KWiHVvWSgW5iEtmiCAj1tE6YvFB3_tovjuCMcb5J9mlfqmuwEUnntr4jrRWzFr33Xvw4NKN3UUAzUDHB50GfbsKfPRdCuDdfJ-mHxueKIGu8zJhqD3GNqPYUupkYaMPfuVCAIM3EQFVGIVd05P15hj-zdp2VlN_TCJ-J93v14y_JV6w96IVBHg3gqMAlsm4G6hS-JWaP9YtAADGKhNlaOXUrMHbBT5rH2YliaEQ5bCkL-ejHZrMIZ9gEbmlXzz8Oi3Zu1RlXs4OtJMsjXniOu16K4rjjBJzm70TJCmsiAYntVGhjC-BjvMXSyvY59nVfSygQ7irVXznDomEIqZzFA73p_VjfC0FjTXpuv90Y3SRh4KMGcvs5Rlnmx-hGD-RnW3eo8tEvYn4ztixfWmN8-AZ_0_OHbNqu-zeRyM_shPdXxN_CGtybWM=w1312-h94-no)

Wow dari 900-an MB menjadi 8.89 MB saja. Demikian artikel yang saya share semoga bermanfaat :)