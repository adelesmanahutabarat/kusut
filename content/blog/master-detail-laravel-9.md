---
title: "Insert Master dan Detail Kedalam Database"
date: 2022-02-25T10:07:47+06:00
draft: false

# post thumb
image: "images/featured-post/laravel/master-detail.png"

# meta description
description: "Insert Master dan Detail Kedalam Database"

# taxonomies
categories:
  - "Laravel"
tags:
  - "Laravel"
  - "jQuery"
  - "PHP"

# post type
type: "featured"
---

<hr>

Untuk melakukan insert Data Master dan Detail dengan laravel 9, kita akan menggunakan jQuery untuk memudahkan kita membuat kolom detail pada interface halaman web Kita.

#### 1. Buat Project Laravel


Pertama kita buat dulu project laravelnya dengan perintah berikut.

> composer create-project --prefer-dist laravel/laravel blog

**blog** disana adalah nama project kita, kalau mau rubah bisa sesuai keinginan kamu.

#### 2. Set Koneksi Database

Buka project dengan IDE yang kamu punya seperti Sublime, Visual Studio Code atau IDE lain.
Sebelum membuat koneksi silahkan buat satu database dan beri nama blog.

Buka file .env dan ubah koneksi mysql kamu seperti dibawah berikut.

```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=blog
DB_USERNAME=root
DB_PASSWORD=
```

#### 3. Create Tabel dengan Migration

Kita akan membuat 2 tabel, yaitu tabel penjualan dan penjualan detail.
1. penjualans = Master
2. penjualandetails = Detail

Lakukan perintah berikut untuk membuat table

> php artisan make:migration create_penjualans_table

> php artisan make:migration create_penjualandetails_table

Ubah file migration penjualan pada **blog/database/migrations/2022_02_25_095305_create_penjualans_table.php**

```
Schema::create('penjualans', function (Blueprint $table) {
    $table->id();
    $table->string('no_faktur');
    $table->string('nama_pelanggan');
    $table->string('tgl_pembelian');
    $table->timestamps();
});
```

Dan penjualan_detail pada **blog/database/migrations/2022_02_25_095305_create_penjualandetails_table.php**

```
Schema::create('penjualandetails', function (Blueprint $table) {
    $table->id();
    $table->string('no_faktur');
    $table->string('nama_barang');
    $table->integer('qty');
    $table->double('harga');
    $table->timestamps();
});
```
Setelah mengubah kedua file diatas, jalankan perintah berikut:

> php artisan migrate


#### 3. Buat Model

Jalankan perintah berikut untuk membuat Model.

> php artisan make:model Penjualan

> php artisan make:model Penjualandetail

Ubah code Model **blog/app/Penjualan**

```
    use HasFactory;
    protected $fillable = [
       'no_faktur',
       'nama_pelanggan',
       'tgl_pembelian', 
   ];
```

Dan **blog/app/Penjualandetail**

```
    use HasFactory;
    protected $fillable = [
     'no_faktur',
     'nama_barang',
     'qty',
     'harga',
 ];
```


#### 3. Buat Controller
Buat controller dengan perintah berikut:

> php artisan make:controller PenjualanController

Setelah itu ketikkan code berikut pada file **blog/app/http/PenjualanController**:

```
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use App\Models\Penjualan;
use App\Models\Penjualandetail;


class PenjualanController extends Controller
{
    public function index()
    {
        $penjualan = Penjualan::all();
        return view("penjualan.index", ['penjualan' => $penjualan]);
    }

    public function add()
    {
        return view('penjualan.add');
    }

    public function store(Request $request)
    {           
        $penjualan = Penjualan::create([
            'no_faktur' =>  $request->no_faktur,
            'nama_pelanggan' =>  $request->nama_pelanggan,
            'tgl_pembelian' => $request->tgl_pembelian,
        ]);

        $nama_barang = $request->input('nama_barang', []);
        $qty = $request->input('qty', []);
        $harga = $request->input('harga', []);

        for ($i=0; $i < count($nama_barang); $i++) {
            if ($nama_barang[$i] != '') {
                Penjualandetail::create([
                    'no_faktur' => $request->no_faktur,
                    'nama_barang' => $nama_barang[$i],
                    'qty' => $qty[$i],
                    'harga' => $harga[$i],
                ]);
            }
        }
        return redirect('/')->with(['success' => "Data Berhasil Disimpan"]);
    }

    public function edit($id)
    {
        $penjualan = Penjualan::where('no_faktur', $id)->first();
        $penjualandetail = Penjualandetail::where('no_faktur', $id)->get();

        return view('penjualan.edit', ['penjualan' => $penjualan, 'penjualandetail' => $penjualandetail]);
    }

    public function update(Request $request, $id)
    {
        // Update Data Master
        $penjualan = Penjualan::where('no_faktur', $id)->first();
        $penjualan->nama_pelanggan = $request->nama_pelanggan;
        $penjualan->tgl_pembelian = $request->tgl_pembelian;
        $penjualan->save();

        //Hapus detail sebelumnya
        Penjualandetail::where('no_faktur', $id)->delete();

        //Insert Detail Baru        
        $nama_barang = $request->input('nama_barang', []);
        $qty = $request->input('qty', []);
        $harga = $request->input('harga', []);

        for ($i=0; $i < count($nama_barang); $i++) {
            if ($nama_barang[$i] != '') {
                Penjualandetail::create([
                    'no_faktur' => $request->no_faktur,
                    'nama_barang' => $nama_barang[$i],
                    'qty' => $qty[$i],
                    'harga' => $harga[$i],
                ]);
            }
        }
        return redirect('/')->with(['success' => "Data Berhasil Diupdate"]);
    }

    public function destroy($id)
    {
        Penjualan::where('no_faktur', $id)->delete();
        Penjualandetail::where('no_faktur', $id)->delete();

        return redirect('/')->with(['success' => "Data Berhasil Dihapus"]);
    }
}
```

#### 4. Layout dan View

Create folder layouts pada **C:\laragon\www\blog\resources\views**

kemudian buat file **app.blade.php** pada folder layouts tersebut, isi codenya seperti dibawah berikut:

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css">
	<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js"></script>
	<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"></script>
	<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/js/bootstrap.bundle.min.js"></script>
	<title>Master Detail - Benang Kusut</title>
	<style>
		h3{
			text-align: center;
		}
		.aksi{
			width: 15%;
		}
	</style>
</head>
<body>
	<br>
	<h3>Master Detail Penjualan Barang - Benang Kusut</h3>

	@yield('content')

</body>
</html>
```

Create folder penjualan pada **C:\laragon\www\blog\resources\views**

kemudian pada folder tersebut buat 3 file berikut:

**index.blade.php**

```
@extends('layouts.app')
@section('content')
<div class="container">
	<a href="add" class="btn btn-primary">Tambah Data</a>
	<br>
	<br>

	<div class="container-fluid" style="padding: auto;">
		@if ($message = Session::get('success'))
		<div class="alert alert-info alert-block">
			<button type="button" class="close" data-dismiss="alert">×</button> 
			<strong>{{ $message }}</strong>
		</div>
		@endif
	</div>

	<table class="table table-bordered table-hover">
		<thead>
			<tr>
				<th>No</th>
				<th>Nomor Faktur</th>
				<th>Tgl Pembelian</th>
				<th>Aksi</th>
			</tr>
		</thead>
		<tbody>

			@foreach($penjualan as $item)
			<tr>
				<td>{{$loop->iteration}}</td>
				<td>{{$item->no_faktur}}</td>
				<td>{{$item->tgl_pembelian}}</td>
				<td class="aksi">
					<a href="{{$item->no_faktur}}/edit" class="btn btn-success">Ubah</a>
					<a href="{{$item->no_faktur}}/delete" class="btn btn-danger">Hapus</a>
				</td>
			</tr>
			@endforeach
		</tbody>
	</table>
</div>
@endsection
```

**add.blade.php**

```
@extends('layouts.app')
@section('content')

<div class="container">
	<h2>Form Penjualan</h2>
	<form action="store" method="post">
		<div class="form-group">
			<label for="no_faktur">No. Faktur:</label>
			<input type="text" class="form-control" id="no_faktur" placeholder="Nomor Faktur" name="no_faktur" required>
		</div>
		<div class="form-group">
			<label for="nama_pelanggan">Nama Pelanggan:</label>
			<input type="text" class="form-control" id="nama_pelanggan" placeholder="Nama Pelanggan" name="nama_pelanggan" required>
		</div>
		<div class="form-group">
			<label for="tgl_pembelian">Tanggal:</label>
			<input type="date" class="form-control" id="tgl_pembelian" name="tgl_pembelian" required>
		</div>

		<a href="#" class="btn btn-primary pull-right add-record" data-added="0"><i class='uil uil-plus'></i> Add Detail</a>
		<br>
		<br>


		<div class="table-responsive">
			<table class="table table-bordered" id="tbl_posts">
				<thead>
					<tr>
						<th>#</th>
						<th>Nama Barang</th>
						<th>Qty</th>
						<th>Harga</th>
						<th>Sub Total</th>
						<th>Action</th>
					</tr>
				</thead>
				<tbody id="tbl_posts_body">
					<tr id="rec-1">
						<td><span class="sn">1</span>.</td>
						<td> 
							<input type="text" class="form-control barang" id="nama_barang" placeholder="Nama Barang" name="nama_barang[]" required>
						</td>
						<td> 
							<input type="number" class="form-control qty" id="qty" placeholder="0" name="qty[]" onkeyup="myfunction()" required>
						</td>
						<td> 
							<input type="text" class="form-control harga" id="harga" placeholder="0" name="harga[]" onkeyup="myfunction()" required>
						</td>
						<td> 
							<input type="text" class="form-control subtotal" id="subtotal" placeholder="0" name="subtotal[]" readonly>
						</td>
						<td>
							<a class="btn btn-xs delete-record" data-id="1"></a>
						</td>
					</tr>
				</tbody>
				<tfoot>
					<tr id="rec-1">
						<td></td>
						<td class="font-weight-bold">Total</td>
						<td><input type="text" class="form-control input-element input-total_qty" id="total_qty" name="total_qty" placeholder="0" readonly></td>
						<td></td>
						<td> <input type="text" class="form-control input-element input-total_harga" id="total_harga" name="total_harga" placeholder="0" readonly></td>
						<td></td>
					</tr>
				</tfoot>
			</table>
		</div>

		{{ csrf_field() }}
		<button type="submit" class="btn btn-primary">Submit</button>
	</form>
</div>
<table id="sample_table" style="display:none;">
	<tr id="">
		<td><span class="sn"></span>.</td>
		<td> 
			<input type="text" class="form-control barang" id="nama_barang" placeholder="Nama Barang" name="nama_barang[]" required>
		</td>
		<td> 
			<input type="number" class="form-control qty" id="qty" placeholder="0" name="qty[]" onkeyup="myfunction()" required>
		</td>
		<td> 
			<input type="text" class="form-control harga" id="harga" placeholder="0" name="harga[]" onkeyup="myfunction()" required>
		</td>
		<td> 
			<input type="text" class="form-control subtotal" id="subtotal" placeholder="0" name="subtotal[]" readonly>
		</td>
		<td class="text-center"><a href="#" class="btn btn-xs delete-record btn-danger" data-id="0">-</a></td>
	</tr>
</table>

<script src="{{ URL::asset('assets/js/custom.js') }}"></script>
@endsection
```

**edit.blade.php**

```
@extends('layouts.app')
@section('content')

<div class="container">
	<h2>Form Edit Data Penjualan</h2>
	<form action="update" method="post">
		<div class="form-group">
			<label for="no_faktur">No. Faktur:</label>
			<input type="text" class="form-control" id="no_faktur" placeholder="Nomor Faktur" name="no_faktur" value="{{$penjualan->no_faktur}}" readonly>
		</div>
		<div class="form-group">
			<label for="nama_pelanggan">Nama Pelanggan:</label>
			<input type="text" class="form-control" id="nama_pelanggan" placeholder="Nama Pelanggan" name="nama_pelanggan" value="{{$penjualan->nama_pelanggan}}" required>
		</div>
		<div class="form-group">
			<label for="tgl_pembelian">Tanggal:</label>
			<input type="date" class="form-control" id="tgl_pembelian" name="tgl_pembelian" value="{{$penjualan->tgl_pembelian}}" required>
		</div>

		<a href="#" class="btn btn-primary pull-right add-record" data-added="0"><i class='uil uil-plus'></i> Add Detail</a>
		<br>
		<br>

		<div class="table-responsive">
			<table class="table table-bordered" id="tbl_posts">
				<thead>
					<tr>
						<th>#</th>
						<th>Nama Barang</th>
						<th>Qty</th>
						<th>Harga</th>
						<th>Sub Total</th>
						<th>Action</th>
					</tr>
				</thead>
				<tbody id="tbl_posts_body">
					<?php 
					$i = 0;
					$total = 0; 
					?>
					@foreach($penjualandetail as $item)
					<?php 
					$i++; 
					$total += ($item->harga * $item->qty);
					?> 
					<tr id="{{'rec-'.$i}}">
						<td><span class="sn">{{$i}}</span>.</td>
						<td> 
							<input type="text" class="form-control barang" id="nama_barang" placeholder="Nama Barang" name="nama_barang[]" value="{{$item->nama_barang}}" required>
						</td>
						<td> 
							<input type="number" class="form-control qty" id="qty" placeholder="0" name="qty[]" onkeyup="myfunction()" value="{{$item->qty}}" required>
						</td>
						<td> 
							<input type="text" class="form-control harga" id="harga" placeholder="0" name="harga[]" onkeyup="myfunction()" value="{{$item->harga}}" required>
						</td>
						<td> 
							<input type="text" class="form-control subtotal" id="subtotal" placeholder="0" name="subtotal[]" value="{{$item->harga * $item->qty}}" readonly>
						</td>
						<td class="text-center"><a href="#" class="btn btn-xs delete-record btn-danger" data-id="{{$i}}">-</a>
						</td>
					</tr>
					@endforeach
				</tbody>
				<tfoot>
					<tr id="rec-1">
						<td></td>
						<td class="font-weight-bold">Total</td>
						<td><input type="text" class="form-control input-element input-total_qty" id="total_qty" name="total_qty" placeholder="0" value="{{ $penjualandetail->sum('qty')}}" readonly></td>
						<td></td>
						<td> <input type="text" class="form-control input-element input-total_harga" id="total_harga" name="total_harga" placeholder="0" value="{{$total}}" readonly></td>
						<td></td>
					</tr>
				</tfoot>
			</table>
		</div>

		{{ csrf_field() }}
		<input type="hidden" name="_method" value="PUT">

		<button type="submit" class="btn btn-primary">Submit</button>
	</form>
</div>
<table id="sample_table" style="display:none;">
	<tr id="">
		<td><span class="sn"></span>.</td>
		<td> 
			<input type="text" class="form-control barang" id="nama_barang" placeholder="Nama Barang" name="nama_barang[]" required>
		</td>
		<td> 
			<input type="number" class="form-control qty" id="qty" placeholder="0" name="qty[]" onkeyup="myfunction()" required>
		</td>
		<td> 
			<input type="text" class="form-control harga" id="harga" placeholder="0" name="harga[]" onkeyup="myfunction()" required>
		</td>
		<td> 
			<input type="text" class="form-control subtotal" id="subtotal" placeholder="0" name="subtotal[]" readonly>
		</td>
		<td class="text-center"><a href="#" class="btn btn-xs delete-record btn-danger" data-id="0">-</a></td>
	</tr>
</table>

<script src="{{ URL::asset('assets/js/custom.js') }}"></script>
@endsection
```

Pada code diatas kita memisahkan javascriptnya supaya hemat code, dimana kita membuat file javascriptnya pada satu file, dengan begitu kita bisa menggunakan script tersebut secara berulang tanpa mengetik ulang script tersebut disetiap halaman.

Create folder dan file seperti berikut **assets/js/custom.js** pada folder **C:\laragon\www\blog\public**, jika sudah maka struktur foldernya akan menjadi seperti berikut: **C:\laragon\www\blog\public\assets\js\custom.js** 

**custom.js**

```
jQuery(document).delegate('a.add-record', 'click', function(e) {
		e.preventDefault();    
		var content = jQuery('#sample_table tr'),
		size = jQuery('#tbl_posts >tbody >tr').length + 1,
		element = null,    
		element = content.clone();
		element.attr('id', 'rec-'+size);
		element.find('.delete-record').attr('data-id', size);
		element.appendTo('#tbl_posts_body');
		element.find('.sn').html(size);
	});

	jQuery(document).delegate('a.delete-record', 'click', function(e) {
		e.preventDefault();    
		var didConfirm = confirm("Are you sure You want to delete row");
		if (didConfirm == true) {
			var id = jQuery(this).attr('data-id');
			var targetDiv = jQuery(this).attr('targetDiv');
			jQuery('#rec-' + id).remove();

			$('#tbl_posts_body tr').each(function(index) {
				$(this).find('span.sn').html(index+1);
			});
			myfunction();
			return true;
		} else {
			return false;
		}
	});

	function myfunction() {
		var sumSubTotal = 0;
		var sumQty = 0;
		var sumTotal = 0;

		var barang  = document.getElementsByClassName('barang');
		var qty  = document.getElementsByClassName('qty');
		var harga  = document.getElementsByClassName('harga');
		var subtotal  = document.getElementsByClassName('subtotal');


		for(var i=0; i<(barang.length); i++) {
			sumSubTotal = parseInt(qty[i].value) * parseFloat(harga[i].value) || 0;
			subtotal[i].value = sumSubTotal;
			sumQty += parseFloat(qty[i].value) || 0;
			sumTotal += parseFloat(subtotal[i].value) || 0;
		}

		document.getElementById("total_qty").value = sumQty;
		document.getElementById("total_harga").value = sumTotal;
	}
```
#### 5. Route

Ubah file **C:\laragon\www\blog\routes\web.php** menjadi seperti berikut:

```
<?php
use App\Http\Controllers\PenjualanController;
use Illuminate\Support\Facades\Route;

Route::get('/', [PenjualanController::class, 'index']); 
Route::get('/add', [PenjualanController::class, 'add']);
Route::post('/store', [PenjualanController::class, 'store']);
Route::get('/{id}/edit', [PenjualanController::class, 'edit']);
Route::put('/{id}/update', [PenjualanController::class, 'update']);
Route::get('/{id}/delete', [PenjualanController::class, 'destroy']);
```

#### 6. Run
Jalankan project laravel dengan penrintah berikut:

> php artisan serve

<hr>

Sekarang kamu sudah bisa testing web kamu.

**Halaman Utama**
![image](../../images/post/master-detail-utama.png)

**Halaman Add**
![image](../../images/post/master-detail-add.png)

**Halaman Edit**
![image](../../images/post/master-detail-edit.png)

Selesai.

Semoga dapat membantu. Sampai jumpa pada tutorial-tutorial berikutnya.
