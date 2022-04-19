---
title: "Autentikasi Dengan Laravel 9"
date: 2022-04-14T10:07:47+06:00
draft: false

# post thumb
image: "images/featured-post/laravel/authentication.png"

# meta description
description: "Cara mudah membuat Autentikasi Laravel 9"

# taxonomies
categories:
  - "Laravel"
tags:
  - "Laravel"
  - "PHP"

# post type
type: "featured"
---

<hr>

Dalam mengembangkan suatu aplikasi yang aman, login merupakan salah satu hal yang wajib kita gunakan. Laravel memudahkan kita untuk membuat Autentikasi atau login untuk mengakses aplikasi kita. 

##### Langkah-langkah Singkat:

1. Buat Project laravel dengan perintah
**composer create-project --prefer-dist laravel/laravel blog**

2. Buat Database dan Daftarkan ke Project
3. Jalankan Perintah
**Composer require laravel/jetstream**
4. **php artisan jetstream:install livewire**
5. **npm install && npm run dev**
6. **php artisan migrate**
7. **php artisan serve**

.

##### Langkah-langkah Lengkap:
#### 1. Install Laravel
Buat Project laravel dengan perintah sebagai berikut:

> composer create-project --prefer-dist laravel/laravel blog

#### 2. Buat Database dan Daftarkan ke Project
Create database dengan nama blog, kemudian daftarkan database tersebut pada project kamu, tepatnya pada file .env seperti berikut:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=blog
DB_USERNAME=root
DB_PASSWORD=
```

#### 3. Jalankan perintah-perintah berikut pada command line kamu
Sebelum menjalankan perintah-perintah dibawah ini, pastikan kamu sudah berada pada direktori project blog pada command line.

> Composer require laravel/jetstream

> php artisan jetstream:install livewire

> npm install && npm run dev

> php artisan migrate

> php artisan serve

buka browser kamu dan jalankan 

> localhost:8000

Jika sudah, maka pada tampilan utama kamu akan ada link login dan register. Kamu sudah bisa langsung menggunakan autentikasi tersebut, silahkan register akun kemudian login.

**Home**
![authentication-benangkusut](../../images/post/auth-home.png)

**Register**
![authentication-benangkusut](../../images/post/auth-register.png)

**Login**
![authentication-benangkusut](../../images/post/auth-login.png)

**Home setelah login**
![authentication-benangkusut](../../images/post/auth-home-after-login.png)

**Profile**
![authentication-benangkusut](../../images/post/auth-profile.png)

### Kesimpulan
Dengan laravel kita tidak perlu lagi membuat Authentication/Autentikasi secara manual, karena dengan cara diatas pembuatan tabel, controller, model dan viewnya sudah dilakukan otomatis dengan perintah-perintah artisan pada command line tadi. Autentikasi tersebut bisa kita custom juga sesuai kebutuhan kita nantinya.

Semoga dapat membantu. Sampai jumpa pada tutorial-tutorial berikutnya.


{{< youtube S7PHEdk_BUY >}}