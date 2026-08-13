# TABEL

```sql 
/*Table structure for table pelanggan */ 
DROP TABLE IF EXISTS pelanggan; 
CREATE TABLE pelanggan ( 
  id_pelanggan varchar(5) NOT NULL, 
  nm_pelanggan varchar(40) NOT NULL, 
  alamat text NOT NULL, 
  telepon varchar(20) NOT NULL, 
  email varchar(50) NOT NULL, 
  PRIMARY KEY  (id_pelanggan) 
) ENGINE=InnoDB DEFAULT CHARSET=latin1 CHECKSUM=1 
DELAY_KEY_WRITE=1 ROW_FORMAT=DYNAMIC; 
 
/*Table structure for table pesan */ 
DROP TABLE IF EXISTS pesan; 
CREATE TABLE pesan ( 
  id_pesan int(5) NOT NULL auto_increment, 
  id_pelanggan varchar(5) NOT NULL, 
  tgl_pesan date NOT NULL, 
  PRIMARY KEY  (id_pesan), 
  KEY id_pelanggan (id_pelanggan), 
  CONSTRAINT pesan_ibfk_1 FOREIGN KEY (id_pelanggan)  
  REFERENCES pelanggan (id_pelanggan) 
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=latin1; 
 
/*Table structure for table produk */ 
DROP TABLE IF EXISTS produk; 
CREATE TABLE produk ( 
  id_produk varchar(5) NOT NULL, 
  nm_produk varchar(30) NOT NULL, 
  satuan varchar(10) NOT NULL, 
  harga decimal(10,0) NOT NULL default '0',
  stock int(3) NOT NULL default '0', 
  PRIMARY KEY  (id_produk) 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 

/*Table structure for table detil_pesan */
DROP TABLE IF EXISTS detil_pesan; 
CREATE TABLE detil_pesan ( 
  id_pesan int(5) NOT NULL, 
  id_produk varchar(5) NOT NULL, 
  jumlah int(5) NOT NULL default '0', 
  harga decimal(10,0) NOT NULL default '0', 
  PRIMARY KEY  (id_pesan,id_produk), 
  KEY FK_detil_pesan (id_produk), 
  KEY id_pesan (id_pesan),
  CONSTRAINT FK_detil_pesan FOREIGN KEY (id_produk)  
  REFERENCES produk (id_produk), 
  CONSTRAINT FK_detil_pesan2 FOREIGN KEY (id_pesan)  
  REFERENCES pesan (id_pesan) 
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

/*Table structure for table faktur */ 
DROP TABLE IF EXISTS faktur; 
CREATE TABLE faktur ( 
  id_faktur int(5) NOT NULL auto_increment, 
  id_pesan int(5) NOT NULL, 
  tgl_faktur date NOT NULL, 
  PRIMARY KEY  (id_faktur), 
  KEY id_pesan (id_pesan), 
  CONSTRAINT faktur_ibfk_1 FOREIGN KEY (id_pesan)  
  REFERENCES pesan (id_pesan) 
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

/*Table structure for table kuitansi */ 
DROP TABLE IF EXISTS kuitansi; 
CREATE TABLE kuitansi ( 
  id_kuitansi int(5) NOT NULL auto_increment, 
  id_faktur int(5) NOT NULL, 
  tgl_kuitansi date NOT NULL, 
  PRIMARY KEY  (id_kuitansi), 
  KEY FK_kuitansi (id_faktur), 
  CONSTRAINT FK_kuitansi FOREIGN KEY (id_faktur)  
  REFERENCES faktur (id_faktur) 
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
```

# INSERT
```sql
INSERT INTO pelanggan (id_pelanggan,nm_pelanggan,alamat,telepon,email) VALUES ('P0001', 'Achmad Solichin','Jakarta Selatan', '0217327762', 'achmatim@gmail.com'),('P0002', 'Agus Rahman', 'Jl H Said, Tangerang', '0217323234', 'agus20@yahoo.com'), ('P0003', 'Doni Damara', 'Jl. Raya Cimone, Jakarta Selatan', '0214394379', 'damara@yahoo.com'), ('P0004', 'Reni Arianti', 'Jl. Raya Dago No 90', '0313493583', 'renren@yahoo.co.id'), ('P0005', 'Dewi Aminah', 'Jl Arjuna No 40', '0314584883', 'aminahoke@plasa.com'), ('P0006', 'Chotimatul M', 'RT 04 RW 02 Kel Pinang sari', '0219249349', 'fixiz@yahoo.co.id');

INSERT INTO pesan (id_pesan, id_pelanggan, tgl_pesan) VALUES
(1, 'P0001', '2008-02-02'),
(2, 'P0002', '2008-02-05'),
(3, 'P0002', '2008-02-10'),
(4, 'P0004', '2008-01-20'),
(5, 'P0001', '2007-12-14');
```

```sql
-- 1. Data Dummy untuk Tabel Pelanggan
INSERT INTO pelanggan (id_pelanggan, nm_pelanggan, alamat, telepon, email) VALUES
('P0001', 'Budi Santoso', 'Jl. Merdeka No. 10, Jakarta', '081234567890', 'budi.santoso@email.com'),
('P0002', 'Siti Aminah', 'Jl. Sudirman No. 45, Bandung', '085678901234', 'siti.aminah@email.com'),
('P0003', 'Ahmad Dani', 'Jl. Pahlawan No. 12, Surabaya', '087890123456', 'ahmad.dani@email.com'),
('P0004', 'Dewi Lestari', 'Jl. Malioboro No. 88, Yogyakarta', '081987654321', 'dewi.lestari@email.com');

-- 2. Data Dummy untuk Tabel Produk
INSERT INTO produk (id_produk, nm_produk, satuan, harga, stock) VALUES
('PR001', 'Laptop ASUS', 'Unit', 7500000, 15),
('PR002', 'Mouse Wireless', 'Pcs', 150000, 50),
('PR003', 'Keyboard Mechanical', 'Pcs', 450000, 30),
('PR004', 'Monitor 24 Inch', 'Unit', 2100000, 10),
('PR005', 'Flashdisk 32GB', 'Pcs', 75000, 100);

-- 3. Data Dummy untuk Tabel Pesan
INSERT INTO pesan (id_pesan, id_pelanggan, tgl_pesan) VALUES
(1, 'P0001', '2026-06-01'),
(2, 'P0002', '2026-06-03'),
(3, 'P0003', '2026-06-05'),
(4, 'P0004', '2026-06-06');

-- 4. Data Dummy untuk Tabel Detil Pesan
INSERT INTO detil_pesan (id_pesan, id_produk, jumlah, harga) VALUES
(1, 'PR001', 1, 7500000),
(1, 'PR002', 2, 150000),
(2, 'PR003', 1, 450000),
(3, 'PR004', 1, 2100000),
(3, 'PR005', 3, 75000),
(4, 'PR002', 1, 150000);

-- 5. Data Dummy untuk Tabel Faktur
INSERT INTO faktur (id_faktur, id_pesan, tgl_faktur) VALUES
(1, 1, '2026-06-02'),
(2, 2, '2026-06-04'),
(3, 3, '2026-06-06'),
(4, 4, '2026-06-07');

-- 6. Data Dummy untuk Tabel Kuitansi
INSERT INTO kuitansi (id_kuitansi, id_faktur, tgl_kuitansi) VALUES
(1, 1, '2026-06-02'),
(2, 2, '2026-06-04'),
(3, 3, '2026-06-06'),
(4, 4, '2026-06-07');
```