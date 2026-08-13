
---
# GROUP BY
```sql
-- belum menggunakan group by
SELECT pesan.id_pesan, pesan.tgl_pesan, detil_pesan.jumlah 
FROM pesan  
INNER JOIN detil_pesan
ON pesan.id_pesan=detil_pesan.id_pesan;

-- setelah menggunakan group by
SELECT pesan.id_pesan, pesan.tgl_pesan, SUM(detil_pesan.jumlah) AS jumlah
FROM pesan  
INNER JOIN detil_pesan
ON pesan.id_pesan=detil_pesan.id_pesan
GROUP BY id_pesan;
```
---
# HAVING
```sql
-- base
SELECT pesan.id_pesan, detil_pesan.id_produk 
FROM pesan  
INNER JOIN detil_pesan
ON pesan.id_pesan=detil_pesan.id_pesan;

-- group by error
SELECT pesan.id_pesan, detil_pesan.id_produk 
FROM pesan  
INNER JOIN detil_pesan
ON pesan.id_pesan=detil_pesan.id_pesan
GROUP BY id_pesan;

-- group by valid
SELECT pesan.id_pesan, COUNT(detil_pesan.id_produk) AS jumlah 
FROM pesan  
INNER JOIN detil_pesan
ON pesan.id_pesan=detil_pesan.id_pesan
GROUP BY id_pesan;

-- without using having
SELECT pesan.id_pesan, COUNT(detil_pesan.id_produk) AS jumlah 
FROM pesan  
INNER JOIN detil_pesan
ON pesan.id_pesan=detil_pesan.id_pesan
WHERE jumlah > 1
GROUP BY id_pesan;
-- aneh? WHERE tidak dapat diterapkan pada fungsi agregrasi seperti COUNT, SUM, AVG dia merujuk pada detil_pesan.jumlah

-- using having
SELECT pesan.id_pesan, COUNT(detil_pesan.id_produk) AS jumlah 
FROM pesan  
INNER JOIN detil_pesan
ON pesan.id_pesan=detil_pesan.id_pesan
GROUP BY id_pesan
HAVING jumlah > 1;
```
---