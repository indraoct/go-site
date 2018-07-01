---
title: "Golang for Beginner Part 1.2: Print Data as JSON API"
date: 2018-06-09T23:43:00+07:00
draft: false
tags : [Golang,MySql,Data,JSON,API,Gorrila Mux,REST]
categories : [Programming,IT]
metaimage : "https://d2yal1mtmg1ts6.cloudfront.net/Rjj4ZjN3MZMBhh6bQFDeNg-_SgSPHq2-IVHaY_uEvOHKmOhRNmphIQTdgmf0aNdgZV_4"
---
![Rest_API](https://d2yal1mtmg1ts6.cloudfront.net/Rjj4ZjN3MZMBhh6bQFDeNg-_SgSPHq2-IVHaY_uEvOHKmOhRNmphIQTdgmf0aNdgZV_4=w100)

Hari ini kita akan belajar mengenai print data DB MySQL ke dalam bentuk web JSON API. Anda bisa fork full codenya disini :
[Full Code](https://github.com/indraoct/go-for-beginner/blob/master/part1/WebAPI.v1.0.go)

Mengingat kembali postingan sebelumnya mengenai print data row dari mysql yakni [Print Data From DB MySQL](/golang-for-beginner-part-1-print-data-from-db-mysql-20180607/).
Kita akan sedikit memodifikasi fungsi-fungsi sebelumnya yakni dengan menambahkan komponen-komponen 
seperti : 

  1. Gorilla Mux --> library 3rd party digunakan untuk routing web
  2. "json/encoding" --> library go untuk mengakomodasi fungsi-fungsi membaca dan menulis data ke dalam bentuk JSON
  3. "net/http" --> library go untuk keperluan fungsi http seperti GET, POST, PUT, dsb.

Bisa anda lihat pada baris kode berikut :
{{< gist indraoct b76f24e3291173151d847823cf0bd6b9 >}}

Jika dijalankan maka akan menghasilkan data berikut : 
```
{
	"status": 1,
	"message": "Success",
	"Data": [
		{
			"sku": "SSI-D01466064-X3-BLA",
			"product_name": "Salyara Plain Casual Big Blouse (XXXL,Black)",
			"stocks": 52
		},
		{
			"sku": "SSI-D01466013-XX-BLA",
			"product_name": "Salyara Plain Casual Big Blouse (XXL,Black)",
			"stocks": 77
		},
		{
			"sku": "SSI-D01401071-LL-RED",
			"product_name": "Zeomila Zipper Casual Blouse (L,Red)",
			"stocks": 76
		},
		{
			"sku": "SSI-D01401064-XL-RED",
			"product_name": "Zeomila Zipper Casual Blouse (XL,Red)",
			"stocks": 44
		},
		
		.........

```

Bagaimana mudah bukan? jika ada pertanyaan langsung bisa comment di bawah ya :) . Sampai jumpa di episode-episode berikutnya.
Happy Coding :)
