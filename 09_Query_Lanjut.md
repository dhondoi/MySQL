
---
# SUB SELECT
```sql
SELECT ... WHERE col=[ANY|ALL] (SELECT ...);
 
SELECT ... WHERE col [NOT] IN (SELECT ...); 
 
SELECT ROW(val1,val2,..) =[ANY] (SELECT col1,col2,..); 
 
SELECT ... WHERE col = [NOT] EXISTS (SELECT ...); 

SELECT ... FROM (SELECT ...) AS name WHERE ...;

-- contoh
SELECT id_pelanggan, nm_pelanggan FROM pelanggan  
WHERE id_pelanggan IN (SELECT id_pelanggan FROM 
pesan);

SELECT id_pesan, jumlah FROM detil_pesan
WHERE jumlah = ( SELECT MAX(jumlah) FROM 
detil_pesan);
```
---
