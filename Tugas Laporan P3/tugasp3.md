# Laporan Praktikum P3 : Manajemen User dan Role di MySQL

## Latar Belakang
Manajemen user dan role dalam sistem basis data merupakan aspek penting dalam mengontrol akses dan keamanan database. Dalam praktik ini, dilakukan berbagai konfigurasi user, role, serta privilege dalam MySQL untuk memahami mekanisme pengelolaan hak akses.

## Problem yang Diangkat
Bagaimana cara membuat, menghapus, serta mengelola hak akses user dalam MySQL secara efisien menggunakan role dan privilege?

## Solusi/Skenario Aktivitas
### 1. Pembuatan User
1. Membuat user sesuai jumlah anggota kelompok:
```sql
CREATE USER 'Hayba'@'localhost' IDENTIFIED BY 'hayba';
CREATE USER 'Kevin'@'localhost' IDENTIFIED BY 'hayba';
CREATE USER 'Joy'@'localhost' IDENTIFIED BY 'hayba';
```
2. Mengecek daftar user:
```sql
SELECT USER, HOST FROM mysql.user;
```
![image](https://github.com/user-attachments/assets/ad51e92e-932c-48da-bb66-9fa10cfaf944)

### 2. Penghapusan User
Menghapus user yang telah dibuat:
```sql
DROP USER 'Kevin'@'localhost';
```
Mengecek kembali daftar user setelah penghapusan:
```sql
SELECT USER, HOST FROM mysql.user;
```
![image](https://github.com/user-attachments/assets/3ffcaff6-07cb-4203-945b-5a6ec99e223f)

### 3. Pembuatan Role
Membuat role untuk memudahkan manajemen hak akses:
```sql
CREATE ROLE 'role_Pardi_select_insert';
CREATE ROLE 'role_Kevin_select_insert';
```
![image](https://github.com/user-attachments/assets/fa650925-cb3c-415e-a9bf-eac42f3d2a64)

### 4. Pemberian Privilege kepada Role
Membuat database dan tabel:
```sql
USE kelompok_1;
CREATE TABLE table1 (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(50) NOT NULL,
    usia INT NOT NULL
);
```
Memberikan hak akses SELECT dan INSERT ke role:
```sql
GRANT SELECT, INSERT ON kelompok_1.* TO 'role_Pardi_select_insert';
GRANT SELECT, INSERT ON kelompok_1.* TO 'role_Kevin_select_insert';
```
![image](https://github.com/user-attachments/assets/6f717326-6147-4bd1-b279-dbfe16b5a3e5)

### 5. Pembuatan Role Tambahan
Role Pertama
```sql
CREATE ROLE 'role_Pardi_create_drop';
```
Role Kedua
```sql
CREATE ROLE 'role_Kevin_create_drop';
```
![image](https://github.com/user-attachments/assets/c55eff7b-cbee-44a3-bfa2-8069176d8685)

### 6. Memberikan privilege create dan drop kedalam role yang telah dibuat sebelumnya dengan menggunakan script
```sql
GRANT CREATE, DROP ON namadatabase.* TO ‘role_nama_anda_creat_drop’;
```
1. Role Pertama
```sql
GRANT CREATE, DROP ON kelompok_1.* TO 'role_Pardi_create_drop';
```
2. Role Kedua
```sql
GRANT CREATE, DROP ON kelompok_1.* TO 'role_Kevin_create_drop';
```
![image](https://github.com/user-attachments/assets/e4f5d371-5a38-4ad3-afaa-8e26622ddd5e)

### 7. Pemberian Role ke User
Pada langkah langkah sebelumnya kita sudah membuat role dengan nama sebagai berikut
```sql
CREATE ROLE 'role_Pardi_select_insert';
CREATE ROLE 'role_Kevin_select_insert';
```
Script 1:
  •	Role role_Pardi_select_insert diberikan kepada pengguna Hayba dengan host localhost.
  •	Role role_Pardi_create_drop diberikan kepada pengguna Joy dengan host localhost.
```sql
GRANT 'role_Pardi_select_insert' TO 'Hayba'@'localhost';
GRANT 'role_Pardi_create_drop' TO 'Joy'@'localhost';
```

![image](https://github.com/user-attachments/assets/361fc979-3811-4c94-ad1e-81bc3677fc25)

Script 2:
  •	Role role_Kevin_select_insert diberikan kepada pengguna Hayba dengan host localhost.
  •	Role role_Kevin_create_drop diberikan kepada pengguna Joy dengan host localhost.
```sql
GRANT 'role_Kevin_select_insert' TO 'Hayba'@'localhost';
GRANT 'role_Kevin_create_drop' TO 'Joy'@'localhost';
```

![image](https://github.com/user-attachments/assets/25e81124-9181-477c-919d-fd6f2e071190)

### 8. Pengujian Hak Akses Sebelum dan Sesudah
```sql
SHOW GRANTS FOR 'Hayba'@'localhost';
SHOW GRANTS FOR 'Joy'@'localhost';
```
![image](https://github.com/user-attachments/assets/c3553ef0-2875-4166-b098-89b23c9afb26)

Perbedaan sebelum dan sesudah pemberian role menunjukkan bahwa GRANT USAGE hanya memberikan akses dasar, sementara pemberian role menambahkan hak akses tertentu.

### 9. Pencabutan Role dari User
```sql
REVOKE 'role_Kevin_select_insert', 'role_Pardi_select_insert' FROM 'Hayba'@'localhost';
REVOKE 'role_Kevin_create_drop', 'role_Pardi_create_drop' FROM 'Joy'@'localhost';
```
![image](https://github.com/user-attachments/assets/0e2c3ae9-05d9-47f5-9211-7d012e278eed)

  •	Perintah 1 mencabut role role_Kevin_select_insert dan role_Pardi_select_insert dari user Hayba.
  •	Setelah eksekusi berhasil, user Hayba tidak lagi memiliki hak akses yang diberikan oleh role tersebut.
  •	Perintah 2 mencabut role role_Kevin_create_drop dan role_Pardi_create_drop dari user Joy.
  •	Setelah eksekusi berhasil, user Joy tidak lagi memiliki hak akses yang diberikan oleh role tersebut.


### 10. Monitoring Aktivitas dengan General Log
```sql
SET GLOBAL general_log = 1;
SET GLOBAL log_output = 'TABLE';
SELECT * FROM kelompok_1.table1;
INSERT INTO kelompok_1.table1 (id, nama, usia) VALUES (175, 'Joshe', 19);
SELECT * FROM mysql.general_log;
```
  
Hasil dari eksekusi script diatas sebagai berikut

  ![image](https://github.com/user-attachments/assets/36b7984e-7f9c-4b4c-aee6-d12c6fbec135)

  ![image](https://github.com/user-attachments/assets/49c237a8-fc18-4675-b14e-1ac315c60f4a)

Penjelasan:
  •	SET GLOBAL general_log = 1; →  Mengaktifkan general log untuk mencatat semua query yang dieksekusi.
  
  •	SET GLOBAL log_output = 'TABLE'; → Menampilkan data dalam tabel dan mencatatnya dalam log sistem.
  
  •	SELECT * FROM kelompok_1.table1; → Menampilkan semua data yang ada dalam tabel table1 dari database kelompok_1.
  
  •	INSERT INTO kelompok_1.table1 (id, nama, usia) VALUES (175, 'Joshe', 19); → Menambahkan data baru dengan ID 175, nama Joshe, dan usia 19 ke dalam tabel table1.
  
  •	Perintah ini digunakan untuk menampilkan semua query yang telah dieksekusi sejak general log diaktifkan.
  
  •	Log ini disimpan dalam tabel mysql.general_log, yang mencatat semua perintah SQL yang dilakukan oleh user di server MySQL.

### 11.	Pengelolaan user, role, dan privilege dalam sistem manajemen basis data adalah aspek krusial untuk mengontrol akses pengguna terhadap database. Dalam praktik ini, dilakukan beberapa langkah penting, antara lain:
  •	Membuat user baru menggunakan perintah CREATE USER.
  
  •	Menghapus user dengan perintah DROP USER.
  
  •	Membuat role menggunakan CREATE ROLE dan menetapkan hak akses dengan GRANT.
  
  •	Menguji hak akses sebelum dan sesudah role diberikan kepada user.
  
  •	Mencabut role dari user menggunakan REVOKE, sehingga user kembali tanpa hak akses tambahan.
  
  •	Melakukan monitoring aktivitas database dengan mengaktifkan general_log.


## Kesimpulan
Pengelolaan user, role, dan privilege dalam sistem basis data sangat penting untuk mengontrol akses. Dalam praktik ini telah dilakukan:
- Pembuatan dan penghapusan user.
- Pembuatan role dan pemberian hak akses.
- Pengujian hak akses sebelum dan sesudah pemberian role.
- Pencabutan role dari user.
- Monitoring aktivitas database dengan general log.

Hasil pengujian menunjukkan bahwa penggunaan role lebih efisien dalam manajemen hak akses dibandingkan pemberian privilege langsung ke user. Selain itu, pencatatan log mempermudah pemantauan operasi dalam database untuk meningkatkan keamanan sistem.

## Referensi
- Dokumentasi MySQL: https://dev.mysql.com/doc/
- Tutorial Basis Data: https://www.w3schools.com/sql/


