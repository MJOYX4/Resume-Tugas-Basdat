# LAPORAN PRAKTIKUM TUGAS P2




## **1. Latar Belakang**
Basis data merupakan elemen penting dalam sistem informasi modern. Instalasi dan konfigurasi database server, seperti MySQL, sangat diperlukan agar sistem dapat berfungsi dengan optimal. Pemahaman terhadap database relasional (RDBMS) dan database non-relasional (NoSQL) sangat penting dalam menentukan sistem penyimpanan data yang paling sesuai dengan kebutuhan aplikasi.

## **2. Problem yang Diangkat**
- Bagaimana cara melakukan instalasi dan konfigurasi MySQL?
- Bagaimana cara mengubah pengaturan default seperti port dan ukuran buffer?
- Bagaimana perbedaan antara database relational dan unrelational?

## **3. Solusi/Skenario Aktivitas**
1. Langkah pertama buka website “Mysql Official Site” untuk nantinya di download, setelah itu klik pada bagian “No thanks, just start my download”
   
    ![image](https://github.com/user-attachments/assets/8d30197a-4880-4e15-9445-438b28f00d41)

2. Setelah itu buka file download tadi,sampai muncul tampilan beranda awal seperti ini. Pilih type “server only” dan klik “Next”
   
    ![image](https://github.com/user-attachments/assets/63e0ea49-2a64-42ff-99f5-14170929039a)

3. Selanjutnya klik lingkaran mirah dan klik ”Execute”, tunggu sebentar sampai 
100% dan akan berubah menjadi ”Next”. Lalu klik ”Next”
   
    ![image](https://github.com/user-attachments/assets/d26a3128-bc03-4c7c-af21-5a7811077edc)

4. Selanjutnya klik “Next”
   
    ![image](https://github.com/user-attachments/assets/abfb49f8-ae19-41ef-8ca9-d4af0774d44f)

5. Setelah itu akan muncul seperti ini dan pilih "Next"
   
    ![image](https://github.com/user-attachments/assets/d4dead8f-9a18-43ec-975f-b9685d035c87)

6. Lalu akan muncul tampilan seperti ini dan klik sampai lingkaran biru lalu klik "Next"
    
    ![image](https://github.com/user-attachments/assets/d643c84a-ccde-4ec8-94a0-971990981ed5)

7. Selanjutnya masukkan pasword lalu klik "Next"

    ![image](https://github.com/user-attachments/assets/2d69c098-a6e8-4e23-9d4a-3e59fb56c37b)

8. Lalu klik "Next" lagi
   
    ![image](https://github.com/user-attachments/assets/a7839870-fbb4-49f8-83fb-9ee4047e4bde)

9. Pilih dan klik "Next"
    
    ![image](https://github.com/user-attachments/assets/31c2563e-403b-4fb4-9c9c-82fd35bbda95)

10. Selanjutnya Pilih "Execute" dan tunggu

    ![image](https://github.com/user-attachments/assets/dfd9eb19-9421-40cd-b9d3-f41347b693d8)

11. Setelah itu klik "Finish"

    ![image](https://github.com/user-attachments/assets/7e36227d-473c-46da-bdfa-22eb7271a759)

12. Setelah itu klik "Next"
    
     ![image](https://github.com/user-attachments/assets/fc64dcf2-9205-4060-b4dc-218447029309)

13. Setelah itu klik "Finish"
    
     ![image](https://github.com/user-attachments/assets/58be230e-0fde-4cee-af8c-824419faf25d)

14. Selanjutnya buka pencarian dan ketikkan ”environment” dan buka
    
     ![image](https://github.com/user-attachments/assets/a0c95aff-2d0a-433d-89df-05a421ae7498)

15. Setelah itu klik pada path, lalu lakukan seperti pada gambar
     
     ![image](https://github.com/user-attachments/assets/97154e20-0fa3-4a52-9e12-384b642b2382)

16. Setelah itu tambahkan path seperti ini 

     ![image](https://github.com/user-attachments/assets/e5179d01-8978-4623-9166-4c6f45e17203)

17. Setelah itu klik ”win + r” dan ketikkan ”%PROGRAMDATA’

     ![image](https://github.com/user-attachments/assets/1bd1d77c-1b70-488b-b07e-f345a3ed87ad)

18. Setelah itu buka klik  ”PROGRAM DATA”, lalu ”MYSQL”, setela itu klik ”my.ini”. Setelah itu ubah agar pada 3306 menjadi 3309

     ![image](https://github.com/user-attachments/assets/fec30ea4-1201-4b72-ae41-2a16e408495c)

19. Setelah itu ubah pada bagian innodb menjadi 25% dari RAM
     ![image](https://github.com/user-attachments/assets/6cb4c092-6391-4b3a-8e12-d52dba763de5)
Menjadi
     ![image](https://github.com/user-attachments/assets/54880704-37e8-487e-b1c6-ab6b904043eb)

20. Setelah itu simpan file dengan “ctrl + s” dan simpan file pada “documents” 
21.	Setelah itu copy file tersebut dan replay pada ”PROGRAM DATA”, lalu ”MYSQL” 
22.	Setelah itu klik kanan pada windows dan pilih ”Terminal Admin” 
23.	Setelah itu login dan masukkan password 

     ![image](https://github.com/user-attachments/assets/1bca4bcd-9b08-4a2a-829a-b85161370425)

24. Untuk mengecek apakah port sudah terganti atau belum bisa dicek dengan kode berikut

     ![image](https://github.com/user-attachments/assets/1c942e02-a124-4a69-a501-31c92f908ad0)

25. Untuk mengecek innodb bisa dicek dengan kode berikut
    
     ![image](https://github.com/user-attachments/assets/277c6e68-36e4-45b6-830b-f2044b34d3da)

26. Setelah itu buat database nya 26.	Setelah itu buat database nya

     ![image](https://github.com/user-attachments/assets/35c753b8-15dd-437e-83f9-01a729054271)
     ![image](https://github.com/user-attachments/assets/ea0815d4-6460-4a97-9653-d4214e20e0e6)

27. Cek apakah data sudah masuk atau belum
    
     ![image](https://github.com/user-attachments/assets/c6f68006-de7a-4a75-8950-31c45c9e1946)

## **4. Pembahasan Singkat dari aktivitas tersebut**
### **4.1 Database Relasional vs. Database Unrelasional**
- **Database Relasional (RDBMS)**
  - Menggunakan tabel dan relasi antar data.
  - Contoh produk: MySQL, PostgreSQL, Microsoft SQL Server.
  
- **Database Unrelasional (NoSQL)**
  - Menggunakan format penyimpanan seperti dokumen atau key-value.
  - Contoh produk: MongoDB, Cassandra, Redis.

### **4.2 Instalasi dan Konfigurasi MySQL**
#### **Langkah Instalasi:**
1. Mengunduh MySQL dari situs resmi.
2. Memilih opsi "Server Only" saat instalasi.
3. Menyelesaikan proses instalasi dengan konfigurasi default.

#### **Konfigurasi Tambahan:**
1. **Mengubah Port Default**
   - Mengedit file `my.ini` dan mengganti `port=3306` menjadi `port=3309`.
   - Menyimpan perubahan dan me-restart server.
   
2. **Mengubah `innodb_buffer_pool_size`**
   - Menyesuaikan ukuran buffer dengan `25%` dari RAM sistem.

3. **Mengubah Password Root**
   - Menggunakan perintah `ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';`
   - Menguji akses dengan login ulang.

### **4.3 Pengujian Konfigurasi**
- Mengecek perubahan port dengan perintah:
  ```
  SHOW VARIABLES LIKE 'port';
  ```
- Mengecek perubahan buffer pool dengan perintah:
  ```
  SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
  ```
- Membuat database uji dan melakukan verifikasi dengan `SHOW DATABASES;`.

## **5. Kesimpulan**
Setelah melakukan instalasi dan konfigurasi MySQL, dapat disimpulkan bahwa:
- Database relasional lebih cocok untuk data dengan struktur tetap, sedangkan NoSQL cocok untuk data yang fleksibel.
- Konfigurasi MySQL dapat disesuaikan untuk meningkatkan kinerja sistem, seperti perubahan port dan buffer pool.
- Pengujian dilakukan untuk memastikan bahwa semua perubahan berhasil diterapkan.

## **6. Referensi**
1. Oracle Corporation. (2021). MySQL Documentation. Retrieved from https://dev.mysql.com/doc/
2. MongoDB Inc. (2021). MongoDB Documentation. Retrieved from https://www.mongodb.com/docs/
3. PostgreSQL Global Development Group. (2021). PostgreSQL Documentation. Retrieved from https://www.postgresql.org/docs/


