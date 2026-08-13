
---
# MENAMBAH RECORD
```sql
INSERT INTO <nama_tabel>(<field1>,<field2>,...,<fieldn>) 
VALUES (<'nilai_field1'>,<'nilai_field2'>,...,<'nilai_fieldn'>);

-- contoh
INSERT INTO pelanggan (id_pelanggan,nm_pelanggan,alamat,telepon,email) VALUES ('P0001', 'Achmad Solichin','Jakarta Selatan', '0217327762', 'achmatim@gmail.com'),('P0002', 'Agus Rahman', 'Jl H Said, Tangerang', '0217323234', 'agus20@yahoo.com'), ('P0003', 'Doni Damara', 'Jl. Raya Cimone, Jakarta Selatan', '0214394379', 'damara@yahoo.com'), ('P0004', 'Reni Arianti', 'Jl. Raya Dago No 90', '0313493583', 'renren@yahoo.co.id'), ('P0005', 'Dewi Aminah', 'Jl Arjuna No 40', '0314584883', 'aminahoke@plasa.com'), ('P0006', 'Chotimatul M', 'RT 04 RW 02 Kel Pinang sari', '0219249349', 'fixiz@yahoo.co.id');
```
---
# MELIHAT RECORD
```sql
SELECT * FROM <nama_tabel>;

-- contoh
SELECT * FROM pelanggan;
```
---
# MENGUBAH RECORD
```sql
UPDATE <nama_tabel> SET <field1>='<nilaibaru>' [WHERE <kondisi>];

-- contoh
UPDATE pelanggan SET alamat='Tangerang' WHERE 
id_pelanggan='P0001';
```
---
# MENGHAPUS RECORD
```sql
DELETE FROM <nama_tabel> [WHERE <kondisi>];

-- contoh
DELETE FROM pelanggan WHERE id_pelanggan='P0005';
```
---