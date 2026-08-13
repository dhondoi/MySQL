
---
# MEMBUAT TABLE
```sql
CREATE TABLE <nama_tabel> ( 
<field1> <tipe>(<panjang>), 
<field2> <tipe>(<panjang>), 
... 
<fieldn> <tipe>(<panjang>), 
PRIMARY KEY (<field_key>) 
); 
-- contoh
CREATE TABLE pelanggan ( 
 id_pelanggan varchar(5) NOT NULL, 
 nm_pelanggan varchar(30) NOT NULL, 
 alamat text, 
 telepon varchar (20), 
 email varchar (50), 
 PRIMARY KEY(id_pelanggan)  
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
```
- TIPE
1. Numeric
- TINYINT (-128 s/d 127)
- SMALLINT (-32.768 s/d 32.767)
- MEDIUMINT (-8.388.608 s/d 8.388.607)
- INT (-2.147.483.648 s/d 2.147.483.647)
- BIGINT (± 9,22 x 10^18)
- FLOAT (-3.402823466E+38 s/d -1.175494351E-38, 0, dan 1.175494351E-38 s/d 3.402823466E+38)
- DOUBLE (-1.79...E+308 s/d -2.22...E-308, 0, dan 2.22...E-308 s/d 1.79...E+308)
- REAL (Merupakan sinonim dari DOUBLE)
- DECIMAL (Merupakan sinonim dari DOUBLE)
2. Date dan Time
- DATE (YYYY-MM-DD)
- TIME (HH:MM:SS)
- DATETIME (YYYY-MM-DD HH:MM:SS)
- YEAR (YYYY)
3. String
- CHAR (0 s/d 255 karakter)
- VARCHAR (0 s/d 65.535)
- TINYTEXT (0 s/d 65.535)
- TEXT (0 s/d 224)
- LONGTEXT (0 s/d 2^32)
4. Lainnya
- ENUM (sampai dengan 65535 string)
- SET (sampai dengan 255 string)
---
# MENAMPILKAN TABEL
```sql
SHOW TABLES;
```
---
# MENAMPILKAN STRUKTUR TABEL
```sql
DESC <nama_tabel>;
-- contoh
DESC pelanggan;
```
---
# MENGUBAH STRUKTUR TABEL
```sql
ALTER TABLE <nama_tabel> <alter_options>;
```
\<alter_options\> :
- ADD <definisi_field_baru>, contoh :
`ALTER TABLE pelanggan ADD tgllahir date NOT NULL;`
- ADD INDEX <nama_index>
- PRIMARY KEY (<field_kunci>), contoh :
`ALTER TABLE pelanggan ADD PRIMARY KEY(id_pelanggan);`
- CHANGE <field_yang_diubah> <definisi_field_baru>, contoh :
- MODIFY <definisi_field>, contoh :
`ALTER TABLE pelanggan MODIFY tgllahir varchar(8) NOT NULL;` 
- DROP <nama_field>, contoh : 
`ALTER TABLE pelanggan DROP tgllahir;`
- RENAME TO <nama_tabel_baru>, contoh : 
`ALTER TABLE RENAME TO customer;`
---
# MENGHAPUS TABEL
```sql
DROP TABLE <nama_tabel>;
-- contoh
DROP TABLE pelanggan;
```
---