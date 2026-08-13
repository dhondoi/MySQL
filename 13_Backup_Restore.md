# Langkah 1: Pastikan Binary Log Aktif di MySQL
Sebelum melakukan apa pun, pastikan server MySQL Anda sudah mengaktifkan fitur pencatatan Binary Log.

1. Masuk ke terminal MySQL:

```SQL
SHOW VARIABLES LIKE 'log_bin';
```
2. Jika hasilnya ON, berarti aman dan siap. Jika OFF, Anda harus mengaktifkannya dengan mengedit file konfigurasi MySQL (my.cnf atau my.ini), lalu tambahkan baris berikut di bawah [mysqld]:

```Ini, TOML
server-id       = 1
log_bin         = /var/log/mysql/mysql-bin.log
expire_logs_days = 7
```
Setelah itu, restart layanan MySQL Anda (sudo systemctl restart mysql).

# Langkah 2: Lakukan Backup dengan Parameter Binlog
Gunakan perintah mysqldump dengan tambahan parameter --source-data=2 (atau --master-data=2 untuk versi MySQL yang lebih lama). Parameter ini otomatis akan mencatat posisi log transaksi saat itu ke dalam file hasil backup.

```Bash
mysqldump --single-transaction --quick --lock-tables=false --source-data=2 -u username -p namadb | gzip > /path/to/backup/backup_$(date +%F_%H-%M-%S).sql.gz
```
- Tips: Buka file backup tersebut nanti (setelah diekstrak) dan lihat beberapa baris pertamanya. Anda akan menemukan catatan seperti ini:
CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000003', MASTER_LOG_POS=481;
Catatan ini adalah "titik nol" sinkronisasi Anda.

1. mysqldump
Fungsi: Utilitas bawaan MySQL untuk mengekspor database menjadi skrip SQL (kumpulan perintah INSERT, CREATE TABLE, dll).

2. --single-transaction
Fungsi: Membuat snapshot data yang konsisten khusus untuk tabel bermesin InnoDB.

Cara kerja: Perintah ini memulai transaksi SQL tanpa mengunci tabel, sehingga aplikasi Anda tetap bisa melakukan tulis/baca (INSERT/UPDATE/DELETE) ke database secara normal saat proses backup berjalan tanpa takut datanya korup atau tidak sinkron.

3. --quick
Fungsi: Mencegah mysqldump memuat seluruh isi tabel (yang jutaan baris itu) ke dalam memori RAM komputer Anda sekaligus.

Cara kerja: Memaksa mysqldump mengambil data baris per baris langsung dari server database. Ini sangat penting agar server tidak mengalami Out of Memory (kehabisan RAM).

4. --lock-tables=false
Fungsi: Mencegah perintah mysqldump mengunci seluruh tabel di akhir proses.

Alasan: Karena kita sudah menggunakan --single-transaction, penguncian tabel secara keseluruhan (global/table locks) sudah tidak diperlukan lagi dan justru berbahaya karena bisa membuat aplikasi tersendat.

5. --source-data=2
Fungsi: Menyisipkan informasi posisi Binary Log (titik transaksi saat backup diambil) ke dalam baris komentar di dalam file hasil backup.

Angka 2: Artinya informasi tersebut disisipkan dalam bentuk komentar SQL (diawali dengan --), sehingga tidak akan dieksekusi sebagai perintah aktif saat Anda nanti melakukan restore, tetapi bisa dibaca oleh manusia atau mesin untuk kebutuhan Point-in-Time Recovery (PITR). (Catatan: Di versi MySQL lama parameter ini bernama --master-data=2).

6. -u username -p namadb
-u username: Username akun MySQL yang memiliki hak akses untuk membaca database.

-p: Meminta MySQL untuk menanyakan password akun tersebut.

namadb: Nama database spesifik yang ingin Anda backup.

7. | (Pipe)
Fungsi: Operator di sistem operasi Linux/Unix untuk menyalurkan hasil keluaran (output) dari perintah di sebelah kiri langsung menjadi masukan (input) untuk perintah di sebelah kanan (dalam hal ini, dikirim ke gzip).

8. gzip
Fungsi: Program kompresi file standar di Linux. Berfungsi mengecilkan ukuran file teks SQL yang tadinya sangat besar (bisa puluhan GB) menjadi file terkompresi berukuran jauh lebih kecil.

9. > /path/to/backup/backup_$(date +%F_%H-%M-%S).sql.gz
>: Mengarahkan hasil akhir file yang sudah dikompres untuk disimpan ke dalam sebuah file di media penyimpanan.

/path/to/backup/: Direktori atau folder tujuan tempat file backup akan ditaruh.

backup_$(date +%F_%H-%M-%S).sql.gz: Penamaan file otomatis agar unik berdasarkan waktu saat itu.

`$(date +%F_%H-%M-%S)` akan menghasilkan format tanggal dan waktu saat ini, contohnya: backup_2026-08-13_18-37-03.sql.gz. Ini sangat berguna agar file backup tidak saling menimpa (overwrite) jika Anda menjadwalkannya secara rutin via Cron Job.

# Langkah 3: Simulasi Ketika Terjadi Kerusakan (Disaster)
Katakanlah database Anda rusak atau terhapus secara tidak sengaja pada pukul 14.00, padahal backup harian Anda diambil pada pukul 00.00. Antara jam 00.00 sampai 14.00, ada ribuan data baru yang masuk.

Berikut cara memulihkannya secara utuh:

1. Restore File Backup Utama (Kondisi Jam 00.00)
Ekstrak dan restore file .sql hasil backup Anda seperti biasa:

```Bash
gunzip < /path/to/backup/backup_file.sql.gz | mysql -u username -p namadb
```
(Pada titik ini, data Anda kembali seperti jam 00.00 malam).

2. Ambil Data Baru Menggunakan Binary Log (PITR)
Untuk mengembalikan data baru yang masuk dari jam 00.00 hingga sebelum kerusakan terjadi, gunakan utilitas mysqlbinlog.

- Cari tahu file binlog aktif saat ini melalui terminal MySQL:

```SQL
SHOW MASTER STATUS;
-- atau
SHOW BINARY LOG STATUS;
```
Misalnya file binlog saat ini adalah mysql-bin.000003.

- Ekstrak dan jalankan ulang (replay) log transaksi dari titik backup sampai sebelum kerusakan terjadi:

```Bash
mysqlbinlog --start-position=481 /var/log/mysql/mysql-bin.000003 | mysql -u username -p namadb
```
Catatan: Nilai --start-position=481 diambil dari informasi --source-data=2 yang tersimpan di file backup Anda tadi.
Anda juga bisa memfilter berdasarkan waktu menggunakan --stop-datetime="2026-06-06 13:59:00" jika ingin memulihkan tepat sebelum sistem error.

Dengan menyelesaikan langkah di atas, database Anda tidak hanya kembali ke posisi saat dibackup, tetapi data baru yang masuk setelahnya berhasil diselamatkan sepenuhnya tanpa ada yang hilang!