# Praktikum 8 - Pemograman Web
```
Riska Puspa Anggraeni Putri
311910440
TI.19.A2
Universitas Pelita Bangsa
```
# LANGKAH - LANGKAH 
## 1. Buka XAMPP dan jalankan Apache dan MySQL
![LANGKAH 1](https://user-images.githubusercontent.com/56241285/120351439-17c8a400-c32a-11eb-9283-85fc469c697d.png)
## 2. Buat Database baru dengan nama `latihan1` dan buat Tabel
```
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
![LANGKAH 2 MEMBUAT TABEL](https://user-images.githubusercontent.com/56241285/120355548-9410b680-c32d-11eb-9c1f-02274865e503.png)
![LANGKAH 2 MEMBUAT TABEL 2](https://user-images.githubusercontent.com/56241285/120355556-95da7a00-c32d-11eb-94ed-78bc83d1f48e.png)
## 3. Tambahkan data Tabel
```
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```
![LANGKAH 3 MENAMBAHKAN DATA](https://user-images.githubusercontent.com/56241285/120355742-bd314700-c32d-11eb-8431-aa034b87f83d.png)
![LANGKAH 3 MENAMBAHKAN DATA 2](https://user-images.githubusercontent.com/56241285/120355745-be627400-c32d-11eb-92cc-1f8b462b7002.png)
## 4. Buat folder `lab8_php_database` pada root directory web server `(d:\xampp\htdocs)`
![LANGKAH 4 MEMBUAT FILE PADA HTDOCS](https://user-images.githubusercontent.com/56241285/120355885-e94cc800-c32d-11eb-9bc9-7b2a3afe45a5.png)
## 5. Buat file php baru dengan nama `koneksi.php` pada directory `(d:\xampp\htdocs\lab8_php_database)`. Ini coding untuk mengkoneksikan database
```
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
![LANGKAH 5 MEMBUAT KONEKSI PHP](https://user-images.githubusercontent.com/56241285/120356249-519ba980-c32e-11eb-9a59-20d25b193ea8.png)
## 6. Buat file php baru dengan nama `index.php` pada directory `(d:\xampp\htdocs\lab8_php_database)`
```
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <div class="main">
            <a href="tambah.php">Tambah Barang</a>
            <table>
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
                <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=$row['nama'];?>"></td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['harga_beli'];?></td>
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
![LANGKAH 6 MEMBUAT INDEX PHP (coding 1)](https://user-images.githubusercontent.com/56241285/120356304-62e4b600-c32e-11eb-9ffe-336459f17479.png)
![LANGKAH 6 MEMBUAT INDEX PHP (coding 2)](https://user-images.githubusercontent.com/56241285/120356308-64ae7980-c32e-11eb-80fa-99e46c937bce.png)
* Hasilnya
![LANGKAH 6 MEMBUAT INDEX PHP](https://user-images.githubusercontent.com/56241285/120356310-65471000-c32e-11eb-9f4e-f8f672afab71.png)
## 7. Buat file php baru dengan nama `tambah.php` pada directory `(d:\xampp\htdocs\lab8_php_database)`
```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_',$file_gambar['name']);
        $destination = dirname(__FILE__) .'/gambar/' . $filename;
        if(move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }
    
    $sql = 'INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}','{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Tambah Barang</title>
</head>
<body>
<div class="container">
    <h1>Tambah Barang</h1>
    <div class="main">
        <form method="post" action="tambah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" style="margin-left: 20px;" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori" style="margin-left: 58px;" >
                    <option value="Komputer">Komputer</option>
                    <option value="Elektronik">Elektronik</option>
                    <option value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" style="margin-left: 40px;" />
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" style="margin-left: 43px;" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" style="margin-left: 85px;" />
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
```
![LANGKAH 7 MENAMBAHKAN BARANG (CODING 1)](https://user-images.githubusercontent.com/56241285/120356372-7b54d080-c32e-11eb-978f-3d56245d45fd.png)
![LANGKAH 7 MENAMBAHKAN BARANG (CODING 2)](https://user-images.githubusercontent.com/56241285/120356378-7d1e9400-c32e-11eb-9484-4b22733f25e0.png)
![LANGKAH 7 MENAMBAHKAN BARANG (CODING 3)](https://user-images.githubusercontent.com/56241285/120356385-7db72a80-c32e-11eb-92de-9c073ad19741.png)
* Hasilnya
![LANGKAH 7 MENAMBAHKAN BARANG](https://user-images.githubusercontent.com/56241285/120356394-7e4fc100-c32e-11eb-9000-56af802dc4d6.png)
![LANGKAH 7 MENAMBAHKAN BARANG 2](https://user-images.githubusercontent.com/56241285/120356390-7e4fc100-c32e-11eb-88c7-d4fc7a18abd1.png)
## 8. Buat file php baru dengan nama `ubah.php` pada directory `(d:\xampp\htdocs\lab8_php_database)`
```
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
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
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
        <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $data['nama'];?>" style="margin-left: 20px;"/>
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori" style="margin-left: 58px;">
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" style="margin-left: 40px;"/>
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" style="margin-left: 43px;"/>
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $data['stok'];?>" style="margin-left: 85px;"/>
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
                    <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
```
![LANGKAH 8 UBAH BARANG (CODING 1)](https://user-images.githubusercontent.com/56241285/120356669-cbcc2e00-c32e-11eb-909f-138a3e9dd0fb.png)
![LANGKAH 8 UBAH BARANG (CODING 2)](https://user-images.githubusercontent.com/56241285/120356680-ccfd5b00-c32e-11eb-80d1-ed4e0507fb8a.png)
![LANGKAH 8 UBAH BARANG (CODING 3)](https://user-images.githubusercontent.com/56241285/120356682-cd95f180-c32e-11eb-9b64-8e2df007ba7b.png)
* Hasilnya
tampilan menu ubah
![LANGKAH 8 UBAH BARANG 3](https://user-images.githubusercontent.com/56241285/120356764-e3a3b200-c32e-11eb-94c7-39efc5e1263d.png)
sebelum diubah
![LANGKAH 8 UBAH BARANG 1](https://user-images.githubusercontent.com/56241285/120356802-ed2d1a00-c32e-11eb-8301-d9427dbe8241.png)
sesudah diubah
![LANGKAH 8 UBAH BARANG 2](https://user-images.githubusercontent.com/56241285/120356816-f0c0a100-c32e-11eb-87b2-6d65e677ec58.png)
## 9. Buat file php baru dengan nama `hapus.php` pada directory `(d:\xampp\htdocs\lab8_php_database)`
```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
![LANGKAH 9 MENGHAPUS BARANG (CODING)](https://user-images.githubusercontent.com/56241285/120356922-0df56f80-c32f-11eb-9d39-cffd400de63b.png)
* Hasilnya
sebelum dihapus
![LANGKAH 9 MENGHAPUS BARANG 1](https://user-images.githubusercontent.com/56241285/120356931-1057c980-c32f-11eb-835b-e405fea2d0d7.png)
sesudah dihapus
![LANGKAH 9 MENGHAPUS BARANG 2](https://user-images.githubusercontent.com/56241285/120356936-10f06000-c32f-11eb-80b5-b56b3f0fc78e.png)
## 10. Tambahkan CSS untuk mempercantik tampilan. Buat file CSS baru dengan nama `style.css` pada directory `(d:\xampp\htdocs\lab8_php_database)`
```
body{
    font-family: sans-serif;
}
table{
    border-collapse: collapse;
}
th, td{
    border: 1px solid black;
    font-size: 16px;
    padding: 7px 9px;
}

/* Tambah Barang */
.input{
    padding: 5px;
}
```
