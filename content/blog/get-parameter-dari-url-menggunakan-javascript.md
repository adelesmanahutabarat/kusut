---
title: "Get Parameter Dari URL Menggunakan Javascript"
date: 2022-04-19T10:07:47+06:00
draft: false

# post thumb
image: "images/featured-post/js/get-parameter-from-url-with-js.png"

# meta description
description: "Get Parameter Dari URL Menggunakan Javascript"

# taxonomies
categories:
  - "Javacript"
tags:
  - "HTML"
  - "Javascript"

# post type
type: "featured"
---

<hr>

Ada kalanya kita membutuhkan parameter yang ada pada url untuk kita gunakan kembali untuk keperluan seperti get datatable dengan ajax ataupun hal lain. Berikut cara mengambil value dari parameter yang ada pada url.


```
let urlString = window.location.href;
let paramString = urlString.split('?')[1];
let queryString = new URLSearchParams(paramString);

console.log(queryString.get('name'));
console.log(queryString.get('nilai'));
```

**Console**
![get-parameter-from-url-with-javascript-benangkusut](../../images/post/get-parameter-from-url-with-javacript.png)

Semoga dapat membantu. Sampai jumpa pada tutorial-tutorial berikutnya.


