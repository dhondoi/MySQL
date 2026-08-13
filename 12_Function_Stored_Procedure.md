
---
# STORED PROCEDURE
- FUNCTION dan PROCEDURE. Perbedaan utama antara function dan procedure adalah terletak pada nilai yang dikembalikannya (di-return). Function memiliki suatu nilai yang dikembalikan (di-return), sedangkan procedure tidak. Umumnya suatu procedure hanya berisi suatu kumpulan proses yang tidak menghasilnya value, biasanya hanya menampilkan saja.
- bentuk umum
```sql
CREATE 
    [DEFINER = { user | CURRENT_USER }] 
    PROCEDURE sp_name ([proc_parameter[,...]]) 
    [characteristic ...] routine_body 
 
CREATE 
    [DEFINER = { user | CURRENT_USER }] 
    FUNCTION sp_name ([func_parameter[,...]]) 
    RETURNS type 
    [characteristic ...] routine_body
```
- contoh sederhana
```sql
-- buat sp
DELIMITER $$ 
CREATE PROCEDURE hello() 
    BEGIN 
    SELECT "Hello World!";   
    END$$
-- balikan fungsi ;     
DELIMITER ;
-- coba panggil
CALL hello();
```
- contoh 1
```sql
DELIMITER $$ 
CREATE PROCEDURE jumlahPelanggan() 
    BEGIN 
     SELECT COUNT(*) FROM pelanggan;   
    END$$ 
DELIMITER ;
CALL jumlahPelanggan();
```
- contoh 2
```sql
DELIMITER $$ 
-- deklarasi variable `hasil`
CREATE PROCEDURE jumlahPelanggan2(OUT hasil INT) 
    BEGIN 
    --  balikkan/simpan nilai output ke `hasil`
     SELECT COUNT(*) INTO hasil FROM pelanggan; 
    END$$ 
DELIMITER ;
CALL jumlahPelanggan2(@hasil);
SELECT @hasil;
```
- contoh 3
```sql
DELIMITER $$ 
-- deklarasi variable `pelanggan` dalam hal ini (id)
CREATE PROCEDURE jumlahItemBarang(pelanggan VARCHAR(5)) 
    BEGIN 
        SELECT SUM(detil_pesan.jumlah) 
        FROM pesan, detil_pesan 
        WHERE pesan.id_pesan=detil_pesan.id_pesan  
        AND pesan.id_pelanggan=pelanggan; 
    END$$ 
DELIMITER ;
CALL jumlahItemBarang('P0001');
```
- contoh 4
```sql
DELIMITER $$ 
-- deklarasi variable `pelanggan` dalam hal ini (id)
CREATE FUNCTION jumlahStockBarang(produk VARCHAR(5))
RETURNS INT
READS SQL DATA 
    BEGIN 
        DECLARE jumlah INT;
        SELECT COUNT(*) INTO jumlah FROM produk
        WHERE id_produk=produk;
        RETURN jumlah; 
    END$$ 
DELIMITER ;
SELECT jumlahStockBarang('PR001');
```
---
# MENGUBAH SP
```sql
ALTER {PROCEDURE | FUNCTION} sp_name  
    [characteristic ...]
```
---
# MENGHAPUS SP
```sql
DROP {PROCEDURE | FUNCTION} [IF EXISTS] sp_name
```
---
# VARIABLE
```sql
-- sintaks
DECLARE variable_name DATATYPE [DEFAULT value];
-- contoh
DECLARE jumlah INT; 
DECLARE kode VARCHAR(5); 
DECLARE tgl_lahir DATE DEFAULT ‘1982-10-20’;
```
---
# SET
```sql
-- sintaks
SET variable_name = expression|value;
-- contoh
SET jumlah = 10; 
SET kode = (SELECT id_pelanggan FROM pelanggan LIMIT 1); 
SET tgl_lahir = now();
-- contoh lainnya
DELIMITER $$ 
CREATE FUNCTION hitungUmur (lahir DATE) 
 RETURNS INT 
    BEGIN 
  DECLARE thn_sekarang, thn_lahir INT; 
  SET thn_sekarang = YEAR(now()); 
  SET thn_lahir = YEAR (lahir); 
  RETURN thn_sekarang - thn_lahir; 
    END$$ 
DELIMITER ;
```
---
# KONDISI
- sintaks
```sql
-- bentuk 1
IF kondisi THEN 
    perintah-jika-benar; 
END IF; 
 
-- bentuk 2
IF kondisi THEN  
    perintah-jika-benar; 
ELSE 
    perintah-jika-salah; 
END IF; 
 
-- bentuk 3
CASE expression 
    WHEN value THEN 
        statements 
    [WHEN value THEN 
        statements ...]
    [ELSE  
        statements] 
END CASE;
```
- contoh
```sql
-- contoh 1
DELIMITER $$ 
CREATE FUNCTION cekPelanggan (pelanggan varchar(5)) 
 RETURNS VARCHAR (100) 
    BEGIN 
  DECLARE jumlah INT; 
  SELECT COUNT(id_pesan) INTO jumlah FROM pesan  
           WHERE id_pelanggan=pelanggan; 
  IF (jumlah > 0) THEN 
   RETURN CONCAT("Anda sudah bertransaksi sebanyak ",  
           jumlah, " kali"); 
  ELSE 
   RETURN "Anda belum pernah melakukan transaksi"; 
  END IF; 
    END$$ 
DELIMITER ;
-- contoh 2
DELIMITER $$ 
CREATE FUNCTION getDiskon(jumlah INT) RETURNS int(11) 
    BEGIN 
  DECLARE diskon INT; 
  CASE  
   WHEN (jumlah >= 100) THEN  
    SET diskon = 10; 
   WHEN (jumlah >= 50 AND jumlah < 100) THEN  
    SET diskon = 5; 
   WHEN (jumlah >= 20 AND jumlah < 50) THEN  
    SET diskon = 3; 
   ELSE SET diskon = 0; 
  END CASE; 
  RETURN diskon; 
    END$$ 
DELIMITER ;
```
---
# PERULANGAN
- sintaks
```sql
-- bentuk 1
[label:] LOOP 
 statements 
END LOOP [label];

-- bentuk 2
[label:] REPEAT 
 statements 
UNTIL expression 
END REPEAT [label] 

-- bentuk 3
[label:] WHILE expression DO 
 statements 
END WHILE [label]
```
- contoh
```sql
SET i=1; 
ulang: WHILE i<=10 DO 
IF MOD(i,2)<>0 THEN  
SELECT CONCAT(i," adalah bilangan ganjil"); 
 END IF; 
 SET i=i+1; 
END WHILE ulang;
```
---