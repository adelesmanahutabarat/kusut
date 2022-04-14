---
title: "Constants Laravel"
date: 2022-03-07T10:07:47+06:00
draft: false

# post thumb
image: "images/featured-post/laravel/constants.png"

# meta description
description: "Constants Laravel"

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

Menggunakan variable konstanta atau constants pada laravel cukup mudah, berikut langkah-langkahnya:

#### 1. Membuat variable constant

Buat 1 file di folder config. Pada contoh ini saya buat nama filenya global.php. Kemudian ketikkan code berikut:

**config\global.php**

```

<?php
   
return [
    'name' => 'Budi',
]
  
?>

```

#### 2. Menggunakan variable
Untuk penggunaan variable constants tersebut cukup mudah, kita tinggal memanggilnya dengan menggunakan perintah **config('global.name')**.

Buat satu controller untuk menampilkan isi variabel constant yang telah kita buat seperti berikut:


**app\Http\Controllers\HomeController.php**

```
public function index()
{
    dd(config('global.wahana_reporting'));
}
```

#### 3. Run

> php artisan serve

Selesai.

Semoga dapat membantu. Sampai jumpa pada tutorial-tutorial berikutnya.


