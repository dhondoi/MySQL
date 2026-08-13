
---
# INNER JOIN
- dengan where
```sql
SELECT tabel1.*, tabel2.* FROM tabel1, tabel2 WHERE tabel1.PK=tabel2.FK;

-- contoh
SELECT pelanggan.id_pelanggan, pelanggan.nm_pelanggan, pesan.id_pesan, pesan.tgl_pesan FROM pelanggan, pesan WHERE pelanggan.id_pelanggan=pesan.id_pelanggan;
```
- dengan inner join
```sql
SELECT tabel1.*, tabel2.* FROM tabel1 INNER JOIN tabel2 ON tabel1.PK=tabel2.FK;

-- contoh
SELECT pelanggan.id_pelanggan, pelanggan.nm_pelanggan, pesan.id_pesan, pesan.tgl_pesan FROM pelanggan INNER JOIN pesan ON pelanggan.id_pelanggan=pesan.id_pelanggan;
```
---
# OUTER JOIN
- dengan left join
```sql
SELECT tabel1.*, tabel2.* FROM tabel1 LEFT JOIN tabel2 ON tabel1.PK=tabel2.FK;

-- contoh
SELECT pelanggan.id_pelanggan, pelanggan.nm_pelanggan, pesan.id_pesan, pesan.tgl_pesan FROM pelanggan LEFT JOIN pesan ON pelanggan.id_pelanggan=pesan.id_pelanggan;
```
- dengan right join
```sql
SELECT tabel1.*, tabel2.* FROM tabel1 RIGHT JOIN tabel2 ON tabel1.PK=tabel2.FK;

-- contoh
SELECT pelanggan.id_pelanggan, pelanggan.nm_pelanggan, pesan.id_pesan, pesan.tgl_pesan FROM pelanggan RIGHT JOIN pesan ON pelanggan.id_pelanggan=pesan.id_pelanggan;
```
---
# TIGA TABEL
```sql
SELECT pesan.id_pesan, produk.id_produk, produk.nm_produk, 
detil_pesan.harga, detil_pesan.jumlah  
FROM pesan, detil_pesan, produk 
WHERE pesan.id_pesan=detil_pesan.id_pesan AND 
detil_pesan.id_produk=produk.id_produk  
AND pesan.id_pesan='1';

SELECT pesan.id_pesan, produk.id_produk, produk.nm_produk, 
detil_pesan.harga, detil_pesan.jumlah FROM pesan INNER JOIN detil_pesan ON pesan.id_pesan=detil_pesan.id_pesan INNER JOIN produk ON detil_pesan.id_produk=produk.id_produk where detil_pesan.id_produk='PR002';
```
---