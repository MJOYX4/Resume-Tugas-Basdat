# LAPORAN PRAKTIKUM P4


---

## Latihan Soal

### 1. Langkah-langkah Import Data

a. Download dan ekstrak dataset employee pada folder.
b. Masuk ke konsol DOS, lalu jalankan perintah berikut:
```sql
USE tugasempat;
SOURCE employee.sql;
```
 ![image](https://github.com/user-attachments/assets/278b7b01-f912-44fe-bee5-ce6ba58f6886)

Penjelasan: Kode SQL ini digunakan untuk memilih database `tugasempat`, menjalankan skrip `employee.sql` untuk membuat struktur database, dan menampilkan hasil eksekusi dengan beberapa peringatan.

Pastikan posisi cursor berada di folder penyimpanan dataset.
c. Masuk ke database MySQL.
d. Lakukan import data dengan menggunakan perintah berikut:
```sql
SOURCE employee.sql;
```

  ![image](https://github.com/user-attachments/assets/6174903d-7f19-425e-81fa-1e7ea1a8d441)

Penjelasan : Eksekusi perintah SQL di MySQL. Perintah USE tugasempat; memilih database, lalu SOURCE employee.sql; menjalankan skrip SQL untuk membuat struktur database. 

e. Tunggu hingga proses import selesai.
f. Setelah selesai, maka nanti pada database, akan tampak sebagai berikut:

  ![image](https://github.com/user-attachments/assets/52c3afec-4207-4a43-bc91-32aa94c87486)


#### Simulasi Penggunaan Index

a. Jalankan query berikut:
```sql
SELECT * FROM employee;
```

  ![image](https://github.com/user-attachments/assets/fe46c928-c34d-49db-9d09-2b3ae2b27745)

Penjelasan: Query ini menampilkan seluruh data dalam tabel `employee`, termasuk kolom `emp_no`, `birth_date`, `first_name`, `last_name`, `gender`, dan `hire_date`.

b. Jalankan perintah `EXPLAIN` untuk melihat performa query:
```sql
EXPLAIN SELECT * FROM employee WHERE first_name = 'Georgi';
```

  ![image](https://github.com/user-attachments/assets/deddfa48-09a7-42b7-8616-7d5a3e2095d6)

Penjelasan: `EXPLAIN` pada query pencarian nama `Georgi` di tabel `employee`. MySQL melakukan full table scan (`type: ALL`), karena tidak ada indeks yang digunakan (`possible_keys: NULL`). Hal ini membuat query kurang optimal untuk dataset besar. Solusi: buat index pada kolom `first_name` agar pencarian lebih cepat.

c. Tambahkan index:
```sql
ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name);
```
  ![image](https://github.com/user-attachments/assets/162b279f-bc80-4ebf-be36-79b5df72b573)

Penjelasan: Index ini mempercepat pencarian berdasarkan `first_name` dan `last_name`.

d. Jalankan kembali `EXPLAIN`:
```sql
EXPLAIN SELECT * FROM employee WHERE first_name = 'Georgi' AND last_name = 'Bahr';
```
  ![image](https://github.com/user-attachments/assets/fc5d39d7-3932-4d1a-8262-2b09c16c9dd0)

Penjelasan : Perintah EXPLAIN SELECT * FROM employee WHERE first_name = 'Georgi' AND last_name = 'Bahr' menunjukkan bahwa query menggunakan indeks idx_full_name, yang berarti pencarian lebih efisien dibandingkan full table scan. Kolom type menunjukkan ref, yang berarti indeks digunakan untuk pencocokan nilai tertentu, dan jumlah baris yang diproses (rows) hanya 1, menunjukkan bahwa query ini sangat optimal.

### 2. Tambahkan kolom nama departemen pada table dept_manager. Dan lakukan update terhadap kolom tersebut

a. Tambahkan kolom `nama_departemen` pada tabel `dept_manager`:
```sql
ALTER TABLE dept_manager ADD COLUMN nama_departemen VARCHAR(255);
```
  ![image](https://github.com/user-attachments/assets/376b698e-ab55-4da7-82f6-2f38e055ddbd)

Penjelasan : Perintah ALTER TABLE dept_manager ADD COLUMN nama_departemen VARCHAR(255); menambahkan kolom baru bernama nama_departemen dengan tipe data VARCHAR(255) ke dalam tabel dept_manager. Kolom ini dapat menyimpan nama departemen hingga 255 karakter

b. Update kolom tersebut dengan data dari tabel `department`:
```sql
UPDATE dept_manager AS dm
JOIN department AS dp ON dm.dept_no = dp.dept_no
SET dm.nama_departemen = dp.dept_name;
```
c. Verifikasi perubahan:
```sql
SELECT * FROM dept_manager;
```
![image](https://github.com/user-attachments/assets/a893fb2f-2f45-4d63-ac02-c68793561be5)
Penjelasan : Perintah UPDATE pada tabel dept_manager menambahkan nama departemen (nama_departement) dengan mencocokkan dept_no dari tabel department. Setelah pembaruan, query SELECT * FROM dept_manager menunjukkan bahwa setiap baris sekarang memiliki kolom nama_departement yang sesuai, misalnya "Marketing", "Finance", dan lainnya.

### 3. Menambahkan Kolom dan Update Data di Tabel `dept_emp`

a. Tambahkan kolom `nama_departemen`:
```sql
ALTER TABLE dept_emp ADD COLUMN nama_departemen VARCHAR(255);
```
![image](https://github.com/user-attachments/assets/0cd50b75-0804-40d3-8415-f7b58242569f)

Penjelasan : Perintah ALTER TABLE dept_emp ADD COLUMN nama_departement VARCHAR(255); menambahkan kolom baru bernama nama_departement dengan tipe data VARCHAR(255) ke dalam tabel dept_emp. Hasil Query 1 OK menunjukkan bahwa perintah berhasil dijalankan tanpa error. 

b. Update kolom dengan data dari tabel `department`:
```sql
UPDATE dept_emp AS de
JOIN department AS dp ON de.dept_no = dp.dept_no
SET de.nama_departemen = dp.dept_name;
```
![image](https://github.com/user-attachments/assets/377f28f7-3201-483b-a061-7e18332e6c73)

Penjelasan : Perintah UPDATE mengisi kolom nama_departement dalam tabel dept_emp dengan nilai dari dept_name berdasarkan kecocokan dept_no dari tabel department. Hasilnya menunjukkan bahwa 331.603 baris berhasil diperbarui.

c. Verifikasi perubahan:
```sql
SELECT * FROM dept_emp;
```
![image](https://github.com/user-attachments/assets/eba7189c-d81c-4fef-8d21-6f29ba2d59fb)

Penjelasan : Hasil query SELECT * FROM dept_emp menunjukkan bahwa kolom nama_departement berhasil diperbarui dengan nama departemen yang sesuai berdasarkan dept_no. Data kini mencerminkan keterkaitan antara karyawan dan departemen mereka.

### 4. Menampilkan Gaji Tertinggi di Departemen `d006`
```sql
SELECT e.first_name, e.last_name, s.amount
FROM employee e
JOIN salary s ON e.emp_no = s.emp_no
JOIN dept_emp d ON e.emp_no = d.emp_no
WHERE d.dept_no = 'd006'
AND s.amount = (SELECT MAX(s2.amount) FROM salary s2 JOIN dept_emp d2 ON s2.emp_no = d2.emp_no WHERE d2.dept_no = 'd006');
```
![image](https://github.com/user-attachments/assets/37d8d0f1-17e5-4dde-a2b4-a61e75acc936)

Penjelasan: Query ini mencari karyawan dengan gaji tertinggi di departemen `d006`.

### 5. Dari database employe yang sudah diimport, tambahkan kolom umur pada table employee.

a. Tambahkan kolom `umur`:
```sql
ALTER TABLE employee ADD COLUMN umur INT;
```
![image](https://github.com/user-attachments/assets/793d1e72-c7ae-4878-b0ff-1d88c78c3c67)

Penjelasan : Query ini menambahkan kolom umur bertipe INT ke dalam tabel employee.

b. Update kolom `umur` berdasarkan tanggal lahir:
```sql
UPDATE employee SET umur = TIMESTAMPDIFF(YEAR, birth_date, CURDATE());
```
![image](https://github.com/user-attachments/assets/e72658b7-fb94-4555-99a9-ad8594732ac3)

c. Verifikasi perubahan:
```sql
SELECT * FROM employee;
```
![image](https://github.com/user-attachments/assets/e750fa5b-2489-4b37-9d62-497ab2f98963)

Penjelasan : Query SELECT * FROM employee menampilkan seluruh data dalam tabel employee, termasuk kolom umur yang baru ditambahkan. Kolom ini berisi usia karyawan yang dihitung berdasarkan birth_date.

### 6. Tambahkan masing-masing jenis index diatas composite index

a. Tambahkan indeks:
```sql
ALTER TABLE employee ADD INDEX idx_name (first_name, last_name);
```
![image](https://github.com/user-attachments/assets/294e8224-faaa-4ccb-827c-4b197cfc2c86)

Penjelasan : Query ini menambahkan indeks idx_name pada kolom first_name dan last_name di tabel employee, yang meningkatkan performa pencarian berdasarkan nama.


```sql
  foreign key index pada table employe
ALTER TABLE dept_emp ADD CONSTRAINT fk_emp FOREIGN KEY (emp_no) REFERENCES employee(emp_no);
```
![image](https://github.com/user-attachments/assets/54c06476-3a59-430e-966d-88be97e41a58)

Penjelasan : Composite index (first_name, last_name) membantu mempercepat pencarian berdasarkan nama lengkap. Foreign key memastikan referensial integrity antara dept_emp dan employees.

### 7. Lakukan pengujian terhadap query berikut, apakah sudah mengakses index atau belum.
```sql
EXPLAIN SELECT * FROM employee
  WHERE first_name = 'Georgi'
```
![image](https://github.com/user-attachments/assets/af6d6545-dfed-4229-8317-32f3ecfc4bc8)

Penjelasan : Sebelum index ditambahkan, MySQL melakukan full table scan, yang terlihat dari kolom key kosong. Setelah index ditambahkan, seharusnya kolom key berisi nama index (idx_name), menandakan bahwa query menggunakan index.

### 8. Lakukan pengujian dari query berikut. Apakah ada perbedaan sebelum dan sesudah ditambahkan index. 

Query untuk menguji:
```sql
SELECT * FROM employee
WHERE first_name = 'Georgi'
AND last_name = 'Bahr'
```
Cara menguji:

• Jalankan query diatas sebanyak 10x. catet waktunya setiap kali dijalankan

• Buat index composite yang bersesuaian dengan query diatas.

• Jalankan query diatas sebanyak 10x. catet waktunya setiap kali dijalankan

• Ambil rata2 sebelum dan sesudah

• Tulis kesimpulanya.

```sql
ALTER TABLE employee DROP INDEX idx_name;
```
![image](https://github.com/user-attachments/assets/4e3fd915-f6c1-4d4e-80b2-52e497f3a704)

```sql
ALTER TABLE dept_emp DROP FOREIGN KEY fk_emp;
```
![image](https://github.com/user-attachments/assets/3beb3227-344d-45a0-ba52-6de41fd3af12)


Sebelum:
```
01	2 ms
02	0 ms
03	0 ms
04	0 ms
05	0 ms
06	0 ms
07	0 ms
08	0 ms
09	0 ms
10	0 ms
Rata-rata: 0.2 ms
```


```sql
ALTER TABLE employee DROP INDEX idx_full_name;
```
![image](https://github.com/user-attachments/assets/4ed730bf-a630-4340-a818-31a86f428481)

Penjelasan : Perintah SQL CREATE INDEX idx_employee_name ON employee (first_name, last_name); yang berhasil dijalankan. Perintah ini membuat indeks pada kolom first_name dan last_name di tabel employee untuk meningkatkan kinerja pencarian data berdasarkan kedua kolom tersebut.

Setelah indeks ditambahkan:
```
01	0 ms
02	0 ms
03	0 ms
04	0 ms
05	0 ms
06	0 ms
07	0 ms
08	0 ms
09	0 ms
10	0 ms
Rata-rata: 0 ms
```

### Kesimpulan
Penggunaan indeks dalam database sangat penting untuk meningkatkan efisiensi pencarian data. Tanpa indeks, pencarian dilakukan secara penuh (full table scan), yang memperlambat kinerja pada tabel besar. Dengan indeks, pencarian menjadi lebih cepat dan efisien karena sistem dapat langsung mengakses data yang relevan. Namun, indeks juga memiliki kelemahan karena memperlambat operasi `INSERT`, `UPDATE`, dan `DELETE`. Oleh karena itu, indeks sebaiknya digunakan secara bijak pada kolom yang sering digunakan dalam pencarian.

