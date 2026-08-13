
---
# TRIGGER
- bentuk umum
```sql
CREATE TRIGGER name  
[BEFORE|AFTER] [INSERT|UPDATE|DELETE]  
ON tablename  
FOR EACH ROW statement
```
- NEW digunakan untuk mengambil record yang akan diproses (insert atau update), sedangkan OLD digunakan untuk mengakses record yang sudah diproses (update atau delete).
- contoh
```sql
-- buat tabel log
CREATE TABLE penjualan.log (
    id INT AUTO_INCREMENT PRIMARY KEY,
    description TEXT,
    `datetime` DATETIME,
    user_id VARCHAR(100)
);
-- 1
DELIMITER $$ 
-- 2
CREATE TRIGGER penjualan.before_insert 
BEFORE INSERT ON penjualan.pelanggan  
FOR EACH ROW 
BEGIN 
    INSERT INTO `log` (description, `datetime`, user_id)  
    VALUES (CONCAT('Insert data ke tabel pelanggan id_plg = ', NEW.id_pelanggan), NOW(), USER()); 
END$$ 
-- 3
DELIMITER ;
```
- menghapus trigger
```sql
-- bentuk umum
DROP TRIGGER tablename.triggername;
-- contoh
DROP TRIGGER penjualan.before_insert;
```
---
# VIEW
- bentuk umum
```sql
CREATE 
    [OR REPLACE] 
    [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}] 
    [DEFINER = { user | CURRENT_USER }] 
    [SQL SECURITY { DEFINER | INVOKER }] 
VIEW view_name [(column_list)] 
AS select_statement 
    [WITH [CASCADED | LOCAL] CHECK OPTION]
```
- contoh
```sql
-- 1
CREATE VIEW `data_plg` AS 
(select id_pelanggan, nm_pelanggan, telepon  
from `pelanggan` order by `nm_pelanggan`);

-- tes 1
SELECT * FROM data_plg;

-- 2
CREATE VIEW lap_jumlah_brg_transaksi 
AS  
(SELECT pesan.id_pesan, pesan.tgl_pesan,   
pelanggan.id_pelanggan, pelanggan.nm_pelanggan, 
detil_pesan.jumlah 
FROM pesan, detil_pesan, pelanggan 
WHERE pesan.id_pesan=detil_pesan.id_pesan AND 
pelanggan.id_pelanggan=pesan.id_pelanggan 
GROUP BY pesan.id_pesan);

-- tes2
SELECT * FROM lap_jumlah_brg_transaksi;
```
- alter
```sql
ALTER VIEW `data_plg` AS (select * from `pelanggan` order by `nm_pelanggan`);
```
- hapus
```sql
DROP VIEW data_plg;
```