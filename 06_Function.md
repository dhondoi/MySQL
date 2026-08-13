
---
# CONCAT
```sql
SELECT <field1>, CONCAT(<field2>,' ',<field3>) AS <nama_field_baru> FROM <nama_tabel>;

-- contoh
SELECT nm_pelanggan, CONCAT(alamat,' ',telepon) AS kontak FROM pelanggan;

SELECT nm_pelanggan, CONCAT_WS(".",alamat,telepon) AS kontak FROM pelanggan;
```
---
# SUBSTRING
```sql
SELECT <field1>,  SUBSTRING(<field2>,<awal>,<jumlah>) AS <nama_field_baru> FROM <nama_tabel>;

-- contoh
SELECT id_pelanggan, SUBSTRING(nm_pelanggan,1,4) AS nama FROM pelanggan;
```
---
# LENGTH
```sql
SELECT <field1>,  LENGTH(<field2>) AS <nama_field_baru> FROM <nama_tabel>;

-- contoh
SELECT nm_pelanggan, LENGTH(nm_pelanggan) AS jumlah_karakter_nm_pelanggan FROM pelanggan;
```
---
# TRIM
```sql
SELECT <field1>,  TRIM(<field2>) AS <nama_field_baru> FROM <nama_tabel>;

-- contoh
SELECT TRIM(' Budi Luhur ');
```
---
# LOWER / UPPER
```sql
SELECT <field1>,  [LOWER|UPPER](<field2>) AS <nama_field_baru> FROM <nama_tabel>;

-- contoh
SELECT LOWER('MySQL');
```
---
# TANGGAL
```sql
SELECT NOW();
SELECT MONTH(NOW());
SELECT WEEK(NOW());
SELECT YEAR(NOW());
SELECT HOUR(NOW());
SELECT MINUTE(NOW());
SELECT SECOND(NOW());
# interval
SELECT DATE_ADD(now(), INTERVAL 1 DAY);
```
---
# DATE FORMAT
Tabel berikut berisi panduan untuk pemformatan tanggal dan waktu yang dapat digunakan dalam aplikasi.

| Penanda | Deskripsi | Rentang / Contoh |
| :--- | :--- | :--- |
| `%M` | Nama bulan | Januari ... Desember |
| `%W` | Nama hari (lengkap) | Minggu ... Sabtu |
| `%D` | Urutan hari dalam sebulan | 1 ... 31 |
| `%Y` | Tahun (4 digit) | 2026 |
| `%y` | Tahun (2 digit) | 26 |
| `%a` | Nama hari (singkatan) | Min ... Sab |
| `%H` | Jam (format 24 jam) | 00 ... 23 |
| `%i` | Menit | 00 ... 59 |
| `%s` | Detik | 00 ... 59 |

---
```sql
SELECT DATE_FORMAT(now(), '%d-%M-%Y %H:%i:%s');
```