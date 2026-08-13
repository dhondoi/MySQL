
---
# TRANSAKSI
```sql
-- 1. Mulai transaksi
START TRANSACTION;

-- 2. Ambil data sekaligus KUNCI baris produk dengan id = 10
SELECT * FROM pelanggan WHERE id_pelanggan = 'P0001' FOR UPDATE;
-- (Misal stok terbaca 5)

-- User A sedang mikir / memproses aplikasi...
-- 3. Update data berdasarkan hasil bacaan
UPDATE pelanggan SET nm_pelanggan = 'Budi' WHERE id_pelanggan = 'P0001';

-- 4. Selesai, simpan permanen (kunci dilepas otomatis)
COMMIT;
```
---
# GRANT
- menambahkan user `monty` yang dapat mengakses semua db dari komputer `localhost` dengan password `qwerty` dan berhak menjalankan perintah `GRANT` ke user lain
```sql
GRANT ALL PRIVILEGES ON *.* TO monty@localhost IDENTIFIED BY 'qwerty' WITH GRANT OPTION;
```
- menambahkan user `adinda`, tidak dapat akses database (*.*), hanya dapt mengakses dari komputer dengan ip '192.168.1.5  dan password 'qwerty'
```sql
GRANT USAGE ON *.* TO adinda@192.168.1.5 IDENTIFIED BY 'qwerty';
``` 
- Menambahkan user baru dengan nama user `admin`, hanya dapat mengakses database `mysql`, hanya dapat mengakses dari komputer ‘localhost’ dan dengan password `qwerty`.
```sql
GRANT ALL PRIVILEGES ON mysql.* TO admin@localhost IDENTIFIED BY 'qwerty';
```
- Mengubah hak akses user `adinda` agar dapat mengakses database `penjualan`.
```sql
GRANT ALL PRIVILEGES ON penjualan.* TO adinda@192.168.1.5;
FLUSH PRIVILEGES;
```
- Mengubah hak akses user ‘admin’ agar dapat CREATE di database `penjualan`.
```sql
GRANT CREATE ON penjualan.* TO admin@localhost;
FLUSH PRIVILEGES;
```
---
# REVOKE
- Menghapus hak akses user `admin` terhadap database `penjualan`.
```sql
REVOKE CREATE ON penjualan.* FROM admin@localhost;
FLUSH PRIVILEGES;
```
---
# GANTI PASSWORD
```sql
UPDATE user SET Password = PASSWORD(`123`) WHERE User = `admin` AND Host = `localhost`;
SET PASSWORD FOR admin@localhost = PASSWORD (`123`);
FLUSH PRIVILEGES;
```
---