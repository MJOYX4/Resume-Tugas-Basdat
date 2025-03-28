# **LAPORAN PRAKTIKUM TUGAS**


---

### **Studi Kasus: Partisi Data Transaksi Berdasarkan Kode Wilayah**
#### **• Membuat Tabel Transactions dengan Partisi**
```sql
CREATE TABLE transactions (
    id INT NOT NULL AUTO_INCREMENT,
    transaction_date DATE NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    region_id INT NOT NULL,
    PRIMARY KEY (id, region_id)
)
PARTITION BY LIST (region_id) (
    PARTITION p_jakarta VALUES IN (1), -- Wilayah Jakarta
    PARTITION p_surabaya VALUES IN (2), -- Wilayah Surabaya
    PARTITION p_bandung VALUES IN (3), -- Wilayah Bandung
    PARTITION p_other VALUES IN (4,5,6,7) -- Wilayah lainnya
);
```
![image](https://github.com/user-attachments/assets/205a4914-fb34-4d86-8008-dba2c128c947)


#### **• Menambahkan Data ke Tabel**
```sql
INSERT INTO transactions (transaction_date, amount, region_id) VALUES
('2024-03-01', 500000, 1), -- Jakarta (p_jakarta)
('2024-03-02', 750000, 2), -- Surabaya (p_surabaya)
('2024-03-03', 200000, 3), -- Bandung (p_bandung)
('2024-03-04', 300000, 5), -- Wilayah Lainnya (p_other)
('2024-03-05', 450000, 6); -- Wilayah Lainnya (p_other)
```
![image](https://github.com/user-attachments/assets/7be0b31f-a9ec-4266-b9c0-66a0763dd28f)


#### **• Mengecek Partisi yang Digunakan**
```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'transactions';
```
![image](https://github.com/user-attachments/assets/5c4acd26-6509-42bf-bba2-d3500dc83e8e)


#### **• Menjalankan Query dengan Optimasi Partisi**
```sql
SELECT * FROM transactions WHERE region_id = 2;
```
![image](https://github.com/user-attachments/assets/7f8418ad-5632-40de-b95a-5dea99cdfd0b)


#### **• •	Jika kita ingin menambahkan wilayah baru, misalnya region_id = 8 untuk Semarang, kita bisa menambahkan partisi baru**
```sql
ALTER TABLE transactions
ADD PARTITION (PARTITION p_semarang VALUES IN (8));
```
![image](https://github.com/user-attachments/assets/cce01a70-a1ac-4761-88d3-ce9c038fc8b1)


#### **• Menghapus Partisi Lama (p_bandung)**
```sql
ALTER TABLE transactions
DROP PARTITION p_bandung;
```
![image](https://github.com/user-attachments/assets/2427706e-c0d8-49d4-8096-0fccfbc396ad)

---

## **LATIHAN STUDI KASUS**

### **3. Redesain Tabel tr_penjualan dengan Partisi**
#### **a) Membuat Tabel tr_penjualan_partisi**
```sql
CREATE TABLE tr_penjualan_partisi (
    tgl_transaksi DATETIME DEFAULT NULL,
    kode_cabang VARCHAR(10) DEFAULT NULL,
    kode_kasir VARCHAR(10) DEFAULT NULL,
    kode_item VARCHAR(7) DEFAULT NULL,
    kode_produk VARCHAR(12) DEFAULT NULL,
    jumlah_pembelian INT(11) DEFAULT NULL,
    nama_kasir VARCHAR(40) DEFAULT NULL,
    harga INT(6) DEFAULT NULL
)
PARTITION BY RANGE (YEAR(tgl_transaksi)) (
    PARTITION p0 VALUES LESS THAN (2008),
    PARTITION p1 VALUES LESS THAN (2009),
    PARTITION p2 VALUES LESS THAN (2010),
    PARTITION p3 VALUES LESS THAN (2011),
    PARTITION p4 VALUES LESS THAN (2012),
    PARTITION p5 VALUES LESS THAN (2013),
    PARTITION p6 VALUES LESS THAN (2014),
    PARTITION p7 VALUES LESS THAN (2015)
);
```
![image](https://github.com/user-attachments/assets/67c658d1-86fa-4ed6-8767-f9a392349053)


#### **b) Mengisi Data ke Tabel tr_penjualan_partisi**
- Insert data untuk tahun 2008
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga)
SELECT tgl_transaksi, kode_cabang, kode_kasir, kode_item, kode_produk, jumlah_pembelian, harga
FROM tr_penjualan;

```
![image](https://github.com/user-attachments/assets/b3b027ed-4793-45eb-888e-c1864a76d4aa)

- Insert data untuk tahun 2009
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 1 YEAR), kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga
FROM tr_penjualan;
```
![image](https://github.com/user-attachments/assets/5cf3338f-3b79-4177-9189-d3320ac68de2)

- Insert data untuk tahun 2010
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 3 YEAR), kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga
FROM tr_penjualan;
```
![image](https://github.com/user-attachments/assets/61a3b854-58f9-49b1-b3c9-d789a5494950)


- Insert data untuk tahun 2011
```sql
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 2 YEAR), kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga
FROM tr_penjualan;
```
-- Insert data untuk tahun 2012
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 4 YEAR), kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga
FROM tr_penjualan;

-- Insert data untuk tahun 2013
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga)
SELECT DATE_ADD(tgl_transaksi, INTERVAL 5 YEAR), kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga
FROM tr_penjualan;

-- Insert data untuk tahun 2014
INSERT INTO tr_penjualan_partisi (tgl_transaksi, kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga)

```sql
SELECT DATE_ADD(tgl_transaksi, INTERVAL 6 YEAR), kode_cabang, kode_kasir, kode_item,
kode_produk, jumlah_pembelian, harga
FROM tr_penjualan;
```
![image](https://github.com/user-attachments/assets/5251253f-e0e4-4f8b-9f0d-220c05825415)

---

#### **c) Mengecek Partisi pada tr_penjualan_partisi**

```sql
SELECT TABLE_NAME, PARTITION_NAME, TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'tr_penjualan_partisi';
```
![image](https://github.com/user-attachments/assets/4e34c1d7-a06b-417d-a116-d1a06ba3de7f)


## **4. Membuat Tabel tr_penjualan_raw**
```sql
CREATE TABLE tr_penjualan_raw (
    id_penjualan INT AUTO_INCREMENT PRIMARY KEY,
    tgl_transaksi DATE NOT NULL,
    kode_cabang VARCHAR(10) NOT NULL,
    kode_kasir VARCHAR(10) NOT NULL,
    kode_item VARCHAR(10) NOT NULL,
    kode_produk VARCHAR(10) NOT NULL,
    jumlah_pembelian INT NOT NULL,
    harga DECIMAL(10,2) NOT NULL
);
```
![image](https://github.com/user-attachments/assets/05940bfc-c2e8-4754-bcc5-1bcd3adab398)


### **Pengujian Kinerja Partisi vs Non-Partisi**
```sql
SELECT * FROM tr_penjualan_raw
WHERE tgl_transaksi > DATE('2010-08-01')
AND tgl_transaksi < DATE('2011-07-31');
```
![image](https://github.com/user-attachments/assets/23306324-e350-43de-a5b6-270b3dacacdf)

```sql
SELECT * FROM tr_penjualan_partisi
WHERE tgl_transaksi > DATE('2010-08-01')
AND tgl_transaksi < DATE('2011-07-31');
```

| No | Raw | Partisi |
|----|-----|---------|
| 1  | 7s  | 6s      |
| 2  | 6s  | 4s      |
| 3  | 7s  | 5s      |
| 4  | 6s  | 5s      |
| 5  | 5s  | 3s      |
| 6  | 5s  | 5s      |
| 7  | 5s  | 4s      |
| 8  | 6s  | 5s      |
| 9  | 6s  | 5s      |
| 10 | 7s  | 6s      |
| **Total** | **68s** | **53s** |
| **Rata-rata** | **6.18s** | **4.82s** |

---

## **KESIMPULAN**
Penggunaan partisi tabel terbukti meningkatkan kinerja database dibandingkan dengan tabel tanpa partisi. Rata-rata waktu eksekusi query pada tabel tanpa partisi (raw) adalah **6.18 detik**, sedangkan tabel dengan partisi hanya membutuhkan **4.82 detik**. Hal ini menunjukkan bahwa partisi dapat menghemat waktu pencarian secara signifikan.

Namun, jika pencarian dilakukan berdasarkan kolom selain **tgl_transaksi**, kinerja tabel partisi tidak meningkat secara signifikan. Oleh karena itu, penggunaan indeks tambahan sangat disarankan untuk menjaga performa sistem.

Kesimpulannya, teknik partisi tabel sangat efektif jika digunakan untuk pencarian berdasarkan kolom yang dijadikan dasar partisi. Dengan strategi optimasi yang sesuai, sistem database dapat bekerja lebih efisien dan responsif.

