**LAPORAN PRAKTIKUM TUGAS**


---
 
### **1. Membuat Tabel Orders dan Menambahkan Data**  
```sql
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    product_name VARCHAR(255),
    quantity INT,
    order_date DATETIME
);
INSERT INTO orders (customer_id, product_name, quantity, order_date)
VALUES
    (1001, 'Laptop', 1, '2024-03-01 10:00:00'),
    (1002, 'Mouse', 2, '2024-03-02 11:30:00'),
    (1003, 'Keyboard', 1, '2024-03-03 09:15:00');
```

### **2. Menambahkan Indeks untuk Optimasi Query**  
```sql
CREATE INDEX idx_customer_id ON orders(customer_id);
```

### **3. Mengecek dan Mengoptimalkan Query Cache**  
```sql
SHOW VARIABLES LIKE 'query_cache%';
```
1. Sebelum ![image](https://github.com/user-attachments/assets/17db83af-a06f-420d-81d0-bf1d7c0967a9)

2. Sesudah ![image](https://github.com/user-attachments/assets/8f4ce017-7fed-4f2d-8ff7-b197baf770db)


### **4. Mengecek dan Menambah Partisi**  
```sql
SELECT * FROM transactions WHERE region_id = 2;

ALTER TABLE transactions
ADD PARTITION (PARTITION p_semarang VALUES IN (8));
```
![image](https://github.com/user-attachments/assets/70f00148-f6a3-4792-8141-0aa2e0278be6)

### **5. Menambahkan Partisi Baru**  
```sql
ALTER TABLE transactions
ADD PARTITION (PARTITION p_semarang VALUES IN (8));
```
![image](https://github.com/user-attachments/assets/6fe87fe6-a6f9-45ae-a4c5-11118a62d06b)


### **6. Menghapus Partisi Lama**  
```sql
ALTER TABLE transactions
DROP PARTITION p_bandung;
```
![image](https://github.com/user-attachments/assets/22362fbc-81da-4838-8652-c8d5ee7fc8cb)

---

## **Tugas Optimasi Query**  
### **1. Menambahkan Indeks untuk Menghindari Full Table Scan**  

#### **a. Menambahkan Indeks Tunggal**  

```sql
CREATE INDEX idx_tgl_transaksi ON tr_penjualan_raw (tgl_transaksi);
CREATE INDEX idx_kode_cabang ON tr_penjualan_raw (kode_cabang);
CREATE INDEX idx_kode_kasir ON tr_penjualan_raw (kode_kasir);
CREATE INDEX idx_kode_produk ON tr_penjualan_raw (kode_produk);
```
![image](https://github.com/user-attachments/assets/2b8ab868-b577-4db9-8393-28f0748897d4)

#### **b.	Mengevaluasi Efektivitas Indeks**
```sql
EXPLAIN SELECT * FROM tr_penjualan_raw WHERE kode_cabang = 'CABANG-039' AND tgl_transaksi = '2008-01-01';
```
![image](https://github.com/user-attachments/assets/8bd5c443-9545-4006-9729-6fe8a9502b02)
Kesimpulannya:
✅ Menambahkan index untuk mempercepat pencarian
✅ Menggunakan Prepared Statements untuk query berulang
✅ Meningkatkan caching dengan InnoDB Buffer Pool
✅ Mengecek efektivitas dengan EXPLAIN

### **c. Optimasi Query dengan Agregasi**
```sql
SELECT * FROM tr_penjualan_raw WHERE YEAR(tgl_transaksi) = 2008;
```
![image](https://github.com/user-attachments/assets/a351faad-6a6c-4e64-af39-dde15b852746)


### **2. Optimasi Query dengan Agregasi**  
```sql
SELECT kode_cabang, SUM(jumlah_pembelian) AS total_penjualan, SUM(harga) AS total_pendapatan
FROM tr_penjualan_raw 
WHERE YEAR(tgl_transaksi) = 2008
GROUP BY kode_cabang;
```
![image](https://github.com/user-attachments/assets/3645c0c0-233d-4264-912f-d928184ff806)

Jadi penjelasan untuk nomer ini 
a.	Menghapus SELECT *
•	Menggunakan hanya kolom yang diperlukan (kode_cabang, jumlah_pembelian, harga)
•	Mengurangi beban jaringan dan pemrosesan data
b.	Menggunakan GROUP BY kode_cabang
Mengelompokkan data berdasarkan kode_cabang, sehingga dapat melihat total penjualan dan pendapatan per cabang.
c.	Menggunakan SUM(jumlah_pembelian) dan SUM(harga)
Melakukan agregasi langsung dalam query untuk menghindari pengolahan data tambahan di aplikasi.


### **3. Optimasi Query Berdasarkan Kode Item**  
```sql
SELECT kode_item, SUM(jumlah_pembelian) AS total_penjualan
FROM tr_penjualan_raw 
WHERE kode_item IN ('ITM-001', 'ITM-002', 'ITM-038')
GROUP BY kode_item;
```
![image](https://github.com/user-attachments/assets/34222fd1-507f-4a01-95ae-0d4217486743)
Penjelasan :
-	Query awal kurang optimal karena mengambil semua kolom (`SELECT *`), tidak menggunakan agregasi, dan berpotensi melakukan full table scan, yang memperlambat eksekusi.  

-	Query optimal lebih efisien karena hanya mengambil kolom yang dibutuhkan (`kode_item` dan total penjualan), menggunakan agregasi (`SUM()`), serta mengelompokkan data dengan `GROUP BY`.  

-	Penggunaan indeks pada `kode_item` dapat meningkatkan performa dengan mempercepat proses pencarian data dan menghindari pemindaian seluruh tabel.  

- Hasil optimasi Query lebih cepat, lebih ringan, dan lebih efisien dalam menghitung total penjualan berdasarkan kode item.


### **4. Optimasi Query Berdasarkan Nama Kasir**  
```sql
SELECT nama_kasir, COUNT(*) AS jumlah_transaksi, SUM(harga) AS total_pendapatan
FROM tr_penjualan_raw 
WHERE nama_kasir LIKE '%Galang%'
GROUP BY nama_kasir;
```
![image](https://github.com/user-attachments/assets/f6a6b90b-6355-4f36-ae56-5a0d2c3073d7)

Penjelasan :
•	Query awal kurang optimal karena menggunakan SELECT *, yang mengambil seluruh kolom tabel meskipun tidak semuanya diperlukan. Selain itu, penggunaan LIKE '%Galang%' tanpa indeks akan menyebabkan full table scan, yang memperlambat pencarian data.

•	Query optimal lebih efisien karena:

•	Menggunakan COUNT(*) untuk menghitung jumlah transaksi per kasir.

•	Menggunakan SUM(harga) untuk menghitung total pendapatan per kasir.

•	Menyusun hasil dengan GROUP BY nama_kasir, yang membantu dalam menganalisis data berdasarkan nama kasir.

•	Hasil optimasi: Query lebih cepat, lebih spesifik, dan lebih ringan dalam pemrosesan data.

### **5. Optimasi Query MAX(harga)**  
```sql
DROP INDEX idx_kode_cabang ON tr_penjualan_raw;

SELECT MAX(harga) FROM tr_penjualan_raw WHERE kode_cabang = 'CABANG-039';

CREATE INDEX idx_kode_cabang ON tr_penjualan_raw(kode_cabang);
```
![image](https://github.com/user-attachments/assets/f2ebfc74-fe97-41a1-b84f-bdc3cc9893cb)

Penjelasan :
•	Query awal kurang optimal karena pencarian berdasarkan kode_cabang tanpa indeks menyebabkan full table scan, sehingga eksekusi query menjadi lambat (4.139 detik).

•	Langkah optimasi yang dilakukan:

•	Menghapus indeks (DROP INDEX) terlebih dahulu untuk menguji performa query tanpa indeks.

•	Menjalankan query SELECT MAX(harga) FROM tr_penjualan_raw WHERE kode_cabang = 'CABANG-039'; untuk melihat waktu eksekusi sebelum optimasi.

•	Menambahkan kembali indeks (CREATE INDEX idx_kode_cabang ON tr_penjualan_raw(kode_cabang);) untuk meningkatkan efisiensi pencarian.

•	Hasil setelah optimasi:

•	Query menjadi lebih cepat karena MySQL dapat menggunakan indeks kode_cabang untuk mempercepat proses pencarian data.

•	Waktu eksekusi seharusnya berkurang secara signifikan dibandingkan sebelumnya.

•	Kesimpulan utama: Menambahkan indeks pada kolom yang sering digunakan dalam WHERE dapat meningkatkan performa query secara signifikan. 


---

## **Kesimpulan**  
Optimasi database melalui penggunaan indeks, partisi, dan query yang efisien terbukti meningkatkan performa pencarian data. Dengan penerapan teknik yang tepat, waktu eksekusi query dapat berkurang secara signifikan, meningkatkan efisiensi pengolahan data dalam sistem basis data.

