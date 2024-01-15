# Lab8web
# Nama : Taufik Eka Albani
# Nim. : 312210347
# Kelas: TI.22.A3

## Instruksi Praktikum
1. Persiapkan text editor misalnya VSCode.
2. Buat folder baru dengan nama lab8_php_database pada docroot webserver (htdocs)
3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.
### Langkah-langkah Praktikum
### Persiapan
#### Untuk memulai membuat aplikasi CRUD sederhana, yang perlu disiapkan adalah database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.
#### Menjalankan MySQL Server Untuk menjalankan MySQL Server dari menu XAMPP Contol.

### Menjalankan MySQL Server
#### Untuk menjalankan MySQL Server dari menu XAMPP Contol.
![Screenshot 2024-01-15 111925](https://github.com/taufikalbani13/Lab1web/assets/115517181/68080876-9859-4844-8a8d-62ad58693b57)
### Mengakses MySQL Client menggunakan PHP MyAdmin
#### Pastikan webserver Apache dan MySQL server sudah dijalankan. Kemudian buka melalui browser: http://localhost/phpmyadmin/
### Membuat Database: Studi Kasus Data Barang
![Screenshot 2024-01-15 111941](https://github.com/taufikalbani13/Lab1web/assets/115517181/08b823a8-be55-4605-8c1c-785a74e36da8)

### Membuat Database
```python
CREATE DATABASE latihan1;
```
### Membuat Tabel
```python
CREATE TABLE data_barang (
    id_barang int(10) auto_increment Primary Key,
    kategori varchar(30),
    nama varchar(30),
    gambar varchar(100),
    harga_beli decimal(10,0),
    harga_jual decimal(10,0),
    stok int(4)
);
```
![Screenshot 2024-01-15 112000](https://github.com/taufikalbani13/Lab1web/assets/115517181/c145ce5c-c94a-4bcd-b4f1-b9634b234db7)
### Menambahkan Data
```python
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```
![Screenshot 2024-01-15 112039](https://github.com/taufikalbani13/Lab1web/assets/115517181/7043c587-acb3-4c39-9d46-4eb2706582bc)
### Membuat Program CRUD
#### Buat folder lab8_php_database pada root directory web server (c:\xampp\htdocs)
![Screenshot 2024-01-15 112100](https://github.com/taufikalbani13/Lab1web/assets/115517181/c74e38e0-4063-4597-85c2-423d8674d39b)
#### Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL: http://localhost/lab8_php_database/
![Screenshot 2024-01-15 112115](https://github.com/taufikalbani13/Lab1web/assets/115517181/e06c5a7d-ae1d-4e10-a9ce-7c8a8466a933)
### Membuat file koneksi database
#### Buat file baru dengan nama ```koneksi.php```
```python
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
  echo "Koneksi ke server gagal.";
  die();
} #else echo "Koneksi berhasil";
?>
```
#### Buka melalui browser untuk menguji koneksi database untuk menyampilkan pesan koneksi berhasil, uncomment pada perintah ```echo “koneksi berhasil”;```
![Screenshot 2024-01-15 112132](https://github.com/taufikalbani13/Lab1web/assets/115517181/aca18047-e9ec-4c43-aa48-5f5e76558b85)

### Membuat file index untuk menampilkan data (Read)
#### Buat file baru dengan nama ```index.php```
```python
<?php
include("koneksi.php");

$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <tr>
            <a href="tambah.php?id=">Tambah Barang</a>
        </tr>

        <div class="main">
            <table border="1" cellpadding="5" cellspacing="0">
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Katagori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td><?= $row['nama'];?></td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_beli'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['stok'];?></td>
                <td>
                    <a href="ubah.php?id=<?= $row['id_barang'];?>">Ubah</a>
                    <a href="hapus.php?id=<?= $row['id_barang'];?>">Hapus</a>
                </td>
            </tr>
            <?php endwhile; else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
            <?php endif; ?>
            </table>
        </div>
    </div>
</body>
</html>
```
![Screenshot 2024-01-15 112148](https://github.com/taufikalbani13/Lab1web/assets/115517181/6e31540c-f409-4246-b988-d8059e8add15)
### Menambah Data (Create)
#### Buat file baru dengan nama ```tambah.php```
```python
<?php
include("koneksi.php");

$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <tr>
            <a href="tambah.php?id=">Tambah Barang</a>
        </tr>

        <div class="main">
            <table border="1" cellpadding="5" cellspacing="0">
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Katagori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td><?= $row['nama'];?></td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_beli'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['stok'];?></td>
                <td>
                    <a href="ubah.php?id=<?= $row['id_barang'];?>">Ubah</a>
                    <a href="hapus.php?id=<?= $row['id_barang'];?>">Hapus</a>
                </td>
            </tr>
            <?php endwhile; else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
            <?php endif; ?>
            </table>
        </div>
    </div>
</body>
</html>
```
![Screenshot 2024-01-15 112203](https://github.com/taufikalbani13/Lab1web/assets/115517181/af31ea1d-bcd2-4a57-a1ea-7c8e6907349a)
### Mengubah Data (Update)
#### Buat file baru dengan nama ```ubah.php```
```python
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
	$id = $_POST['id'];
	$nama = $_POST['nama'];
	$kategori = $_POST['kategori'];
	$harga_jual = $_POST['harga_jual'];
	$harga_beli = $_POST['harga_beli'];
	$stok = $_POST['stok'];
	$file_gambar = $_FILES['file_gambar'];
	$gambar = null;
 
	if ($file_gambar['error'] == 0)
	{
		$filename = str_replace(' ', '_', $file_gambar['name']);
		$destination = dirname(__FILE__) . '/gambar/' . $filename;
		if (move_uploaded_file($file_gambar['tmp_name'], $destination))
		{
			$gambar = 'gambar/' . $filename;;
		}
	}
	$sql = 'UPDATE data_barang SET ';
	$sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
	$sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok 
= '{$stok}' ";
	if (!empty($gambar))
		$sql .= ", gambar = '{$gambar}' ";
	$sql .= "WHERE id_barang = '{$id}'";
	$result = mysqli_query($conn, $sql);
	
	header('location: index.php');
}

$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);

function is_select($var, $val) {
 if ($var == $val) return 'selected="selected"';
 return false;
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<link href="style.css" rel="stylesheet" type="text/css" />
	<title>Ubah Barang</title>
</head>
<body>
<div class="container">
	<h1>Ubah Barang</h1>
	<div class="main">
		<form method="post" action="ubah.php"
enctype="multipart/form-data">
			<div class="input">
				<label>Nama Barang</label>
				<input type="text" name="nama" value="<?php echo 
$data['nama'];?>" />
			</div>
			<div class="input">
	 			<label>Kategori</label>
				<select name="kategori">
	 				<option <?php echo is_select
('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
	 				<option <?php echo is_select
('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
 					<option <?php echo is_select
('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
				</select>
			</div>
			<div class="input">
				<label>Harga Jual</label>
				<input type="text" name="harga_jual" value="<?php echo 
$data['harga_jual'];?>" />
			</div>
			<div class="input">
				<label>Harga Beli</label>
				<input type="text" name="harga_beli" value="<?php echo 
$data['harga_beli'];?>" />
			</div>
			<div class="input">
				<label>Stok</label>
				<input type="text" name="stok" value="<?php echo 
$data['stok'];?>" />
			</div>
			<div class="input">
				<label>File Gambar</label>
				<input type="file" name="file_gambar" />
			</div>
			<div class="submit">
				<input type="hidden" name="id" value="<?php echo 
$data['id_barang'];?>" />
				<input type="submit" name="submit" value="Simpan" />
			</div>
		</form>
	</div>
</div>
</body>
</html>
```
![Screenshot 2024-01-15 112218](https://github.com/taufikalbani13/Lab1web/assets/115517181/86d0c226-fd15-4d38-96c6-cef696a639fc)
### Menghapus Data (Delete)
#### Buat file baru dengan nama ```hapus.php```
```python
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
