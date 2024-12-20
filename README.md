REST API MARKETPLACE SEDERHANA DENGAN LARAVEL 11
-------------------------------------------------

KETENTUAN PROJECT / API
------------------
- Api ini menggunakan Token/JWT
- Setiap API diberikan validasi token/JWT
- Ada 2 aktor disini yaitu merchant & customer
- Merchant dapat create product, update, delete, & dapat melihat customer siapa saja yang membeli
- Customer dapat melihat list produk, add to cart, & checkout
- Setiap transaksi produk diatas 15000 maka customer akan mendapatkan bebas ongkir
- Jika transaksi produk diatas 50000 maka customer akan mendapatkan diskon sebesar 10%
- Diskon per item tidak diberlakukan disini (namun kolom untuk diskon per item sudah disediakanm jika sewaktu2 dibutuhkan). Jadi disini hanya berlaku diskon transaksi setelah checkout & shipping cost pada saat checkout
- Apabila migrasi database gagal, maka bisa langsung diimport SQL nya di folder DB-SQL 

Langkah-langkah menjalankan project
------------------------------------

git clone https://github.com/Aziz-Rahman/backend-marketplace-sederhana-aziz-rahman.git

composer install

php artisan key:generate --ansi

php artisan jwt:secret // generate ulang jwt secret

#Sesuaikan file .env dengan database yang akan dibuat

php artisan migrate

#RUNING ...
php artisan serve atau 

php artisan serve --port=9090 untuk menjalankan di port lain

DAFTAR END POINT YANG TERSEDIA
-------------------------------
#MERCHAN

- POST | http://localhost:9090/api/register
- POST | http://localhost:9090/api/login
- POST | http://localhost:9090/api/merchant/product
- POST | http://localhost:9090/api/merchant/updateProduct
- POST | http://localhost:9090/api/merchant/deleteProduct/4
- GET | http://localhost:9090/api/merchant/viewOrders
- GET | http://localhost:9090/api/logout

#CUSTOMER

- POST | http://localhost:9090/api/register
- POST | http://localhost:9090/api/login
- GET | http://localhost:9090/api/products // (Tanpa Login)
- GET | http://localhost:9090/api/product/2 // (Tanpa Login)
- POST | http://localhost:9090/api/customer/cart // ADD TO CART
- GET | http://localhost:9090/api/customer/cart/cartItems
- GET | http://localhost:9090/api/customer/cart/cartCount // Jumlah cart item
- POST | http://localhost:9090/api/customer/cart/update/2
- POST | http://localhost:9090/api/customer/cart/delete/5
- POST | http://localhost:9090/api/customer/checkout
- GET | http://localhost:9090/api/logout


#REGISTER
-----------
POST | http://localhost:9090/api/register

Contoh Request

Body

Raw (JSON)
```
{
    "name": "Customer Z",
    "email": "customer_z@example.com",
    "password": "password123",
    "role": "customer"
}
```
* role : (merchant atau customer)

Contoh Response:
```
{
    "message": "User registered successfully"
}
```
![alt text](https://github.com/Aziz-Rahman/backend-marketplace-sederhana-aziz-rahman/blob/main/screenshoot/register.png)


#LOGIN
-----------
POST | http://localhost:9090/api/login

Contoh Request

Body

Raw (JSON)
```
{
    "email": "customer_z@example.com",
    "password": "password123"
}
```

Contoh Response:
```
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjkwOTAvYXBpL2xvZ2luIiwiaWF0IjoxNzMyNjI0MTE1LCJleHAiOjE3MzI2Mjc3MTUsIm5iZiI6MTczMjYyNDExNSwianRpIjoiWUNoc2FCbDhNOXVUejlYZyIsInN1YiI6IjIiLCJwcnYiOiIyM2JkNWM4OTQ5ZjYwMGFkYjM5ZTcwMWM0MDA4NzJkYjdhNTk3NmY3In0.A3Vj1-ay5jXu3DjBPe_eeUZLw99lXhieDF6N-hreNrA"
}
```
*Token tsb akan diguNakan untuk proses2 didalam sistem
![alt text](https://github.com/Aziz-Rahman/backend-marketplace-sederhana-aziz-rahman/blob/main/screenshoot/login.png)


#ADD PRODUCT
-------------
POST | http://localhost:9090/api/merchant/product

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

Raw (JSON)
```
{
    "title": "Barang Kiriman",
    "description": "Ini merupakan barang kiriman.",
    "price": 66654,
    "stock": 5
}
```

Contoh Response:
```
{
    "message": "Product created successfully",
    "product": {
        "title": "Barang Kiriman",
        "description": "Ini merupakan barang kiriman.",
        "price": 66654,
        "merchant_id": 3,
        "stock": 5,
        "updated_at": "2024-11-26T12:40:49.000000Z",
        "created_at": "2024-11-26T12:40:49.000000Z",
        "id": 1
    }
}
```

#ALL PRODUCT
--------------
GET | http://localhost:9090/api/products

Contoh Response:
```
[
    {
        "id": 1,
        "merchant_id": 1,
        "image": null,
        "title": "Produk 1 diubah judulnya",
        "description": "Deskripsi produk contoh 1.",
        "price": 93000,
        "stock": 11,
        "created_at": "2024-11-25T12:49:41.000000Z",
        "updated_at": "2024-11-26T04:00:23.000000Z"
    },
    {
        "id": 2,
        "merchant_id": 1,
        "image": null,
        "title": "Produk Contoh 2",
        "description": "Deskripsi produk contoh 2.",
        "price": 68000,
        "stock": 52,
        "created_at": "2024-11-25T12:52:13.000000Z",
        "updated_at": "2024-11-25T12:52:13.000000Z"
    },
    ...
]
```

#DETAIL PRODUK
---------------
GET | http://localhost:9090/api/product/2

Contoh Response:
```
{
    "id": 2,
    "merchant_id": 1,
    "image": null,
    "title": "Produk Contoh 2",
    "description": "Deskripsi produk contoh 2.",
    "price": 68000,
    "stock": 52,
    "created_at": "2024-11-25T12:52:13.000000Z",
    "updated_at": "2024-11-25T12:52:13.000000Z"
}
```

#UPDATE PRODUCT
----------------
POST | http://localhost:9090/api/merchant/updateProduct

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

Raw (JSON)
```
{
    "id": 1,
    "title": "Barang Kiriman diubah !!!",
    "description": "Ini merupakan barang kiriman.",
    "price": 66654,
    "stock": 53
}
```

Contoh Response:
```
{
    "message": "Update product successfully"
}
```


#DELETE PRODUCT
---------------
POST | http://localhost:9090/api/merchant/deleteProduct/2

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Response
```
{
    "message": "Product deleted successfully"
}
```

#VIEW ORDER (CUSTOMER YANG SUDAH ORDER)
---------------------------------------
GET | http://localhost:9090/api/merchant/viewOrders

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Response

![alt text](https://github.com/Aziz-Rahman/backend-marketplace-sederhana-aziz-rahman/blob/main/screenshoot/viewCustomerOrder-merchant.png)


#SHOW PRODUCT
-------------
GET | http://localhost:9090/api/customer/products

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Response
```
[
    {
        "id": 1,
        "merchant_id": 1,
        "image": null,
        "title": "Produk 1 diubah judulnya",
        "description": "Deskripsi produk contoh 1.",
        "price": 93000,
        "stock": 11,
        "created_at": "2024-11-25T12:49:41.000000Z",
        "updated_at": "2024-11-26T04:00:23.000000Z"
    },
    {
        "id": 2,
        "merchant_id": 1,
        "image": null,
        "title": "Produk Contoh 2",
        "description": "Deskripsi produk contoh 2.",
        "price": 68000,
        "stock": 52,
        "created_at": "2024-11-25T12:52:13.000000Z",
        "updated_at": "2024-11-25T12:52:13.000000Z"
    },
...

```

#ADD TO CART
--------------
POST | http://localhost:9090/api/customer/cart

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

Raw (JSON)
```
{
    "product_id": 5,
    "quantity": 1
}
```

Contoh Response:
```
{
    "status": "success",
    "message": "Item added to cart"
}
```
*Jika END POINT ini disending maka quantity akan bertambah untuk produk yang sama dan akan menambah row untuk berbeda produk


#GET CART ITEMS
---------------
GET | http://localhost:9090/api/customer/cartItems

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

(kosong)

Contoh Response:
```
{
    "status": "success",
    "cartItems": [
        {
            "id": 2,
            "customer_id": 4,
            "session_id": null,
            "product_id": 6,
            "quantity": 2,
            "price": "46890.00",
            "discount": 0,
            "created_at": "2024-11-29T15:25:34.000000Z",
            "updated_at": "2024-11-29T15:25:36.000000Z",
            "product": {
                "id": 6,
                "merchant_id": 3,
                "image": null,
                "title": "Barang Bagus nih",
                "description": "Hehehehe hihihihii xxxxxxxxxx.",
                "price": 46890,
                "stock": 19,
                "created_at": "2024-11-26T04:47:27.000000Z",
                "updated_at": "2024-11-26T04:47:27.000000Z"
            }
        },
        ...
    ]
}
```

#UPDATE CART
---------------
POST | http://localhost:9090/api/customer/cart/update/{id}

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

Raw (JSON)
```
{
    "quantity": 3 // update jumlah qty
}
```

Contoh Response:
```
{
    "status": "success",
    "message": "Cart updated",
    "product": "Barang Berbahaya",
    "qty": 3,
    "total": "2541780.00"
}
```

// Jika jumlah quantity yang dimasukan melebihi stock yang tersedia, maka akan mendapatkan response:
```
{
    "status": "fail",
    "message": "Insufficient stock"
}
```

#DELETE CART
-------------
POST | http://localhost:9090/api/customer/cart/delete/{id}

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

(Kosong)

Contoh Response:
```
{
    "status": "success",
    "message": "Cart item removed",
    "productDeleted": "Barang Berbahaya",
    "total": "2541780.00"
}
```

#GET CART COUNT (JUMLAH CART ITEM)
---------------
GET | http://localhost:9090/api/customer/cart/cartCount

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

(Kosong)

Contoh Response:
```
{
    "status": "success",
    "count": "6"
}
```



#CHECKOUT
--------------
POST | http://localhost:9090/api/customer/checkout

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Request

Body

Raw (JSON)
```
{
    "customer_address": "Jl. Kesuksesan No.99",
    "customer_city": "Depok",
    "postal_code": "44456",
    "customer_phone": "0898 5436 8888"
}
```

Contoh Response:
```
{
    "status": "success",
    "message": "Invoice created successfully!",
    "invoice": 6
}
```

#LOGOUT
--------------
GET | http://localhost:9090/api/logout

Headers

Content-Type : application/json

Authorization : bearer [TOKEN]

Contoh Response
```
{
    "message": "Successfully logged out"
}
```


