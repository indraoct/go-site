---
title: "Golang for Beginner Part 2.0: Unit Testing"
date: 2018-06-30T17:34:41+07:00
draft: false
tags : [Golang,Unit Test]
metaimage : "https://lh3.googleusercontent.com/sk3N8UdXdZJuOeVII_OBd3vUhJg0bjWoncxu5sCuhoAO2aL114sxqjfVIJJfo82RD0kn7K76LBbT_UkyTFyte3t3C-sjR-JAjJ1HQc5Rfy56B2olHUf7KEC7OF-iPsV5ysTObyzxWg=w2400"
---

![Unit_Testing](https://lh3.googleusercontent.com/sk3N8UdXdZJuOeVII_OBd3vUhJg0bjWoncxu5sCuhoAO2aL114sxqjfVIJJfo82RD0kn7K76LBbT_UkyTFyte3t3C-sjR-JAjJ1HQc5Rfy56B2olHUf7KEC7OF-iPsV5ysTObyzxWg=w200)

Tidak mudah untuk membuat sebuah sistem software yang handal dan mempertahankannya dalam waktu yang lama. Salah satu cara
untuk mengetahui bahwa _code_ yang sudah kita buat apakah sesuai dengan ekspektasi flow bisnis dan _performance_ adalah melakukan
**Unit Testing**. Unit testing adalah salah satu jenis proses otomatisasi test aplikasi software dengan scope yang paling kecil 
yang bisa di test. Disebut unit karena secara individual dan mandiri (tidak tergantung dengan flow proses yang lain) dapat 
di test sehingga dapat memastikan apakah outputnya sudah sesuai dengan ekspektasi kita.

Di bahasa Go, satu _function_ atau _method_ bisa diartikan sebagai **unit**. 

Mari kita mulai dengan membuat file aplikasi dan file test dalam sebuah folder yang sama dan dengan nama _package_ yang sama.
Saya membuat 1 file aplikasi operasi string sebagai berikut :

{{< gist indraoct c203ec86ac3eb0683109600ce7badcc7 >}}

Saya membuat 2 fungsi operasi string yakni **SwapCase** dan **Reverse** . _Swapcase_ untuk mengubah string yang awalanya upercase ke lowercase 
begitupun sebaliknya mengubah string yang awalnya lowercase menjadi uppercase. _Reverse_ untuk mengubah string supaya urutannya terbalik.

**SwapCase** Ekspektasi :
```
    Before :        
    "Hello, World"
    
    After :     
    "hELLO, wORLD"

```
**Reverse** Ekspektasi :
```
     Before :        
        "Hello, World"
        
     After :     
     "dlroW ,olleH"

```

Mari kita kita buktikan apakah code kita sudah sesuai dengan ekspektasi, yakni dengan membuat 1 file test. Di GO, aturan untuk 
membuat file test yakni dengan cara : [NAMA_FILE]_test.go seperti berikut :

{{< gist indraoct e56973aeb433808dd891c4ec372042c1 >}}

Cara melakukan unit testing bisa dengan perintah seperti berikut:
```
    go test -v
```
Setelah dijalankan, jika aplikasi yang kita jalankan sesuai dengan ekspektasi unit test maka akan dapat hasil sebagai berikut :

![Terminal](https://lh3.googleusercontent.com/9cDvrEu7j6tNXWaBgttG_oaO3cpKHrxyHXM6E1MRoalEt6iL0x3UectssmoekevCl4_xcKvJNKn48_vXo084qaNOHFYY-LJOmzDew7pQMlEqRKVnBdV7a5MCj56ZimBoDmcPZGKXaA=w500)

Source code bisa di clone dari sini : [Full Code](https://github.com/indraoct/go-for-beginner/tree/master/part2)






