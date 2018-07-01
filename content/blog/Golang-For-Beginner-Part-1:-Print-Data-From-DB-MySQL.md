---
title: "Golang for Beginner Part 1: Print Data From DB MySQL"
date: 2018-06-07T21:13:35+07:00
draft: false
tags : [Golang,MySql,Data]
categories : [Programming,IT]
metaimage : "https://indraoctama.com/img/golang_part_1/golang_logo.png"
---
![Golang_logo](/img/golang_part_1/golang_logo.png)
Hari ini kita akan belajar bagaimana menulis data row yang ada di database mysql dengan cara koding dengan bahasa Go.
Pertama-tama silahkan unduh dump mysql disini :

[goforbeginner.sql](https://github.com/indraoct/go-for-beginner/blob/master/goforbeginner.sql)

Buatlah database bernama **"goforbeginner"**, lalu import file goforbeginner.sql tersebut ke db **"goforbeginner"** tersebut. Jika sudah
maka strukturnya akan seperti ini :
![db_structure](/img/golang_part_1/db_structure.png)

Kita akan fokuskan kepada table **products** dengan data yang sudah tersedia sebagai berikut :
![table_products](/img/golang_part_1/table_products.png)

Mari kita buat dulu structure code nya sebagai berikut :
```
import (
	"database/sql"
	"log"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)

//Buat product struct sesuai dengan struktur table products
type Products struct {
	Sku          string    `form:"sku" json:"sku"`
	Product_name string    `form:"product_name" json:"product_name"`
	Stocks       int       `form:"stocks" json:"stocks"`
}

func main(){
// kita akan bermain logic disini

}

```

Tiba saatnya kita bermain di **func main()**

* Initialisasi variable **products** (type struct => Products) dan **arr_products** (type []Products).
   
    ```
        func main(){
            var products Products
            var arr_products []Products
            
            ............................
        }
   ```
   
   Variable **products** berguna untuk mapping data yang terbagi menjadi 3 field yakni : SKU, Product Name, dan Stocks. 
   Sedangkan variable **arr_products** untuk menampung semua row (data bersifat array) yang ada di dalam table **products**.
   
* Mendeklarasikan koneksi dengan database MySQL dengan perantaraan variable **db**.

    ```
         func main(){
                    ...........................
                    
           db, err := sql.Open("mysql","root:@tcp(127.0.0.1:3306)/goforbeginner")
           defer db.Close()
                    
            if(err != nil) {
                log.Fatal(err)
            }
                    
                    ............................
         }

    ```
    Anda dapat menyesuaikan attribute diatas sesuai dengan access credential yang terdapat pada aplikasi MySQL anda. Variable 
    *defer db.Close()* adalah untuk menutup koneksi (tcp) ke mysql yang sudah kita open sebelumnya.
    
* Query ke table products dengan fungsi **db.Query**

    ```
    func main(){
   
        ...........................
        rows, err := db.Query("Select sku,product_name,stocks from products ORDER BY sku DESC")
            if err!= nil {
                log.Print(err)
            }
         ...........................  
          
          }

    ```
    
* Finally fungsi looping dan print data 

    ```
        func main(){
        
        ........................... 
        
        count:=0
        	for rows.Next(){
        		if err := rows.Scan(&products.Sku, &products.Product_name, &products.Stocks); err != nil {
        			log.Fatal(err.Error())
        
        		}else{
        			arr_products = append(arr_products, products)
        			fmt.Println(arr_products[count])
        		}
        		count++
        	}
        
        
        }

    ```

* Jalankan aplikasi go dengan cara :
    ```
        go run PrintDataFromDB.go 
    ```
    Lalu akan tampil data sebagai berikut :
    ```
    {SSI-D01466064-X3-BLA Salyara Plain Casual Big Blouse (XXXL,Black) 52}
    {SSI-D01466013-XX-BLA Salyara Plain Casual Big Blouse (XXL,Black) 77}
    {SSI-D01401071-LL-RED Zeomila Zipper Casual Blouse (L,Red) 76}
    {SSI-D01401064-XL-RED Zeomila Zipper Casual Blouse (XL,Red) 44}
    {SSI-D01401050-MM-RED Zeomila Zipper Casual Blouse (M,Red) 73}
    {SSI-D01326299-LL-NAV Siunfhi Ethnic Trump Blouse (L,Navy) 127}
    {SSI-D01326286-LL-KHA Siunfhi Ethnic Trump Blouse (L,Khaki) 210}
    {SSI-D01326223-MM-KHA Siunfhi Ethnic Trump Blouse (M,Khaki) 209}
    
    .............. dan seterusnya
       
    ```
    
Untuk full code nya silahkan fork disini : [Full Code](https://github.com/indraoct/go-for-beginner/blob/master/part1/PrintDataFromDB.go)


Sekian untuk saat ini, nantikan lagi episode-episode berikutnya mengenai **Go For Beginner**, Happy Coding :)