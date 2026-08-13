
---
# BASE
```sql
SELECT * FROM <nama_tabel>;
```
---
# FIELD
```sql
SELECT <field1>,<field2>,...,<fieldn> FROM <nama_tabel>;

-- contoh
SELECT id_pelanggan, nm_pelanggan FROM pelanggan;
```
---
# WHERE CONDITION
```sql
SELECT [<field1>,...,<fieldn> | *] FROM <nama_tabel> WHERE <field> <operator> <argument> [AND | OR <field> <operator> <argument>];

-- contoh
SELECT id_pelanggan, nm_pelanggan, alamat FROM pelanggan WHERE id_pelanggan = 'P0006';

SELECT id_pelanggan, nm_pelanggan, email 
FROM pelanggan WHERE email LIKE '%yahoo%';
```
\<operator\> :
- =, akan bernilai TRUE jika nilai yang dibandingkan sama.
- != atau <>, akan bernilai TRUE jika nilai yang dibandingkan TIDAK SAMA (berbeda).
- >, akan bernilai TRUE jika nilai yang pertama lebih besar dari nilai kedua.
- >=, akan bernilai TRUE jika nilai yang pertama lebih besar atau sama dengan nilai kedua.
- <, akan bernilai TRUE jika nilai yang pertama lebih kecil dari nilai kedua.
- <=, akan bernilai TRUE jika nilai yang pertama lebih kecil atau sama dengan nilai kedua.
---
# PENGURUTAN (ORDER BY)
```sql
SELECT * FROM <nama_tabel>  ORDER BY <field> [ASC | DESC];

-- contoh
SELECT id_pelanggan, nm_pelanggan FROM pelanggan ORDER BY nm_pelanggan DESC;
```
---
# PEMBATASAN (LIMIT)
```sql
SELECT * FROM <nama_tabel> LIMIT <index_mulai,jumlah_record>;

-- contoh
SELECT id_pelanggan, nm_pelanggan FROM pelanggan ORDER BY nm_pelanggan LIMIT 0,3;
```
---
# JUMLAH (COUNT)
```sql
SELECT COUNT(*) FROM <nama_tabel>;

-- contoh
SELECT COUNT(id_pelanggan)FROM pelanggan;
```
---