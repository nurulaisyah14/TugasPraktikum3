# Tugas Praktikum { Pertemuan ke 10 } <img src=https://qph.fs.quoracdn.net/main-qimg-648763cc041459725b62108f4fdf5b91 width="110px" >


|**Nama**|**NIM**|**Kelas**|**Matkul**|
|----|---|-----|------|
|Nurul Aisyah|312310476|TI.23.A5|Basis Data|

## DAFTAR ISI <br>
| No | Description | Link |
|-----|------|-----|
|1|Soal Praktikum 3|[Click Here](#soal-praktikum-3)|
|2|Tugas Praktikum |[Click Here](#tugas-praktikum)|
|3|Tugas Praktikum 3|[Click Here](#tugas-praktikum-3)|
|4|Evaluasi dan Pertanyaan|[Click Here](#evaluasi-dan-pertanyaan)|
|5|PDF|[Click Here](https://drive.google.com/file/d/1NmJa0T-qU4AtYTB9Hj803KfkaNqm6IKM/view?usp=share_link)|

## Soal Praktikum 3
[Link Soal](https://drive.google.com/file/d/1tz8khbJAnozEn4kv2CYn5A9YA3IxKf3R/view)

### Tugas Praktikum
![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/Perintah%20Tugas.png)

#### Script DDL berdasarkan skema ERD
```sql
CREATE DATABASE praktikum3;
USE praktikum3;

CREATE TABLE Mahasiswa (
  nim VARCHAR(10) PRIMARY KEY,
  nama VARCHAR(50) NOT NULL,
  jenis_kelamin ENUM('Laki-laki', 'Perempuan') NOT NULL,
  tgl_lahir DATE NOT NULL,
  jalan VARCHAR(100) DEFAULT NULL,
  kota VARCHAR(50) NOT NULL,
  kodepos VARCHAR(10) DEFAULT NULL,
  no_hp VARCHAR(20) DEFAULT NULL,
  kd_ds VARCHAR(10) NOT NULL,
  CONSTRAINT FK_DosenWali FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds)
);

CREATE TABLE Dosen (
  kd_ds VARCHAR(10) PRIMARY KEY,
  nama VARCHAR(50) NOT NULL
);

CREATE TABLE MataKuliah (
  kd_mk VARCHAR(10) PRIMARY KEY,
  nama VARCHAR(50) NOT NULL,
  sks INT NOT NULL
);

CREATE TABLE JadwalMengajar (
  kd_ds VARCHAR(10) NOT NULL,
  kd_mk VARCHAR(10) NOT NULL,
  hari ENUM('Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu', 'Minggu') NOT NULL,
  jam TIME NOT NULL,
  ruang VARCHAR(20) NOT NULL,
  PRIMARY KEY (kd_ds, kd_mk, hari, jam),
  CONSTRAINT FK_JadwalDosen FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds),
  CONSTRAINT FK_JadwalMatakuliah FOREIGN KEY (kd_mk) REFERENCES MataKuliah(kd_mk)
);

CREATE TABLE KRSMahasiswa (
  nim VARCHAR(10) NOT NULL,
  kd_mk VARCHAR(10) NOT NULL,
  kd_ds VARCHAR(10) NOT NULL,
  semester VARCHAR(10) NOT NULL,
  nilai DECIMAL(4,2) NOT NULL,
  PRIMARY KEY (nim, kd_mk),
  CONSTRAINT FK_KRSMahasiswa FOREIGN KEY (nim) REFERENCES Mahasiswa(nim),
  CONSTRAINT FK_KRSMataKuliah FOREIGN KEY (kd_mk) REFERENCES MataKuliah(kd_mk),
  CONSTRAINT FK_KRSDosen FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds)
);
```
*Saya buat script baru tapi bisa juga lihat scriptnya di tugas sebelumnya [Praktikum 2](https://github.com/AnggitaRisqiNC/Praktikum-2.git)*

#### Table Mahasiswa pada praktikum sebelumnya
![image](0.png)

### Tugas Praktikum 3
#### 1. Lakukan penambahan data pada table mahasiswa dengan mengisi *kd_ds* yang belum ada pada data dosen.
**Script :**
```sql
INSERT INTO Dosen (kd_ds, nama) VALUES
('DS001', 'Johnny'),
('DS002', 'Jake'),
('DS003', 'Hanny'),
('DS004', 'Karina'),
('DS005', 'Eric');
select * from Dosen;

INSERT INTO Mahasiswa (nim, nama, jenis_kelamin, tgl_lahir, jalan, kota, kodepos, no_hp, kd_ds) VALUES
('11223344', 'Ari Santoso', 'Laki-laki', '1979-08-31', NULL, 'Bekasi', NULL, NULL, 'DS001'),
('11223345', 'Ario Talib', 'Laki-laki', '1999-11-16', NULL, 'Cikarang', NULL, NULL, 'DS002'),
('11223347', 'Lisa Ayu', 'Perempuan', '1996-01-02', NULL, 'Bekasi', NULL, NULL, 'DS003'),
('11223348', 'Tiara Wahidah', 'Perempuan', '1908-02-05', NULL, 'Bekasi', NULL, NULL, 'DS004'),
('11223349', 'Anton Sinaga', 'Laki-laki', '1988-03-10', NULL, 'Cikarang', NULL, NULL, 'DS005');
select * from Mahasiswa;
```

**Output Tabel Dosen**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/1.png)

**Output Tabel Mahasiswa**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/2.png)

#### 2. Hapus satu record data pada table dosen yang telah dirujuk pada tabel mahasiswa.
```sql
DELETE FROM Dosen WHERE kd_ds = 'DS002';
```

**Output**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/3.png)

**Keterangan :** Terjadi ERROR dikarenakan `kd_ds` pada tabel Mahasiswa merupakan **FOREIGN KEY** dari tabel refensinya yaitu tabel Dosen. Dan pada tabel Dosen `kd_ds` merupakan **PRIMARY KEY**. Itu artinya, tabel Dosen sebagai tabel *parent/references* dan Mahasiswa sebagai tabel *child* maka dari itu saat menghapus satu record data pada tabel dosen terjadi error. 

#### 3. Ubah mode menjadi *ON UPDATE CASCADE ON DELETE RESTRICT*
```sql
ALTER TABLE Mahasiswa DROP FOREIGN KEY FK_DosenWali;
ALTER TABLE Mahasiswa ADD CONSTRAINT FK_DosenMahasiswa FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds) ON UPDATE CASCADE ON DELETE RESTRICT;
```

**Output**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/4.png)

#### 4. Lakukan perubahan data pada table dosen *(kd_ds)*
```sql
UPDATE Dosen SET kd_ds = 'DS007' WHERE kd_ds = 'DS005';
```

**Output**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/5.png)

**Keterangan :** `kd_ds` dapat diubah dikarenakan sebelumnya menggunakan `ON UPDATE CASCADE`

#### 5. Lakukan penghapusan data pada table dosen
```sql
DELETE FROM Dosen WHERE kd_ds = 'DS001';
```

**Output**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/6.png)

**Keterangan :** Terjadi ERROR dan `kd_ds` tidak dapat dihapus.

#### 6. Ubah mode menjadi *ON UPDATE CASCADE ON DELETE SET NULL*
```sql
ALTER TABLE Mahasiswa DROP FOREIGN KEY FK_DosenMahasiswa;
ALTER TABLE Mahasiswa ADD CONSTRAINT FK_DosenWali FOREIGN KEY (kd_ds) REFERENCES Dosen(kd_ds) ON UPDATE CASCADE ON DELETE SET NULL;
```

**Output**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/7.png)

#### 7. Lakukan penghapusan data pada table dosen
```sql
DELETE FROM Dosen WHERE kd_ds = 'DS004';
```

**Output pada tabel Dosen**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/9.png)


**Output pada tabel Mahasiswa**

![image](https://github.com/AnggitaRisqiNC/Praktikum-3/blob/main/screenshot/8.png)


**Keterangan :** `kd_ds` berhasil dihapus.


 ## Evaluasi dan Pertanyaan
  * Tulis semua perintah-perintah SQL percobaan di atas beserta outputnya!

  ### Syntax SQL

  - Membuat foreign key

    - Dalam ALTER TABLE:
      ```SQL
      ALTER TABLE mahasiswa
      ADD CONSTRAINT fk_dosenwali FOREIGN KEY (kd_ds)     REFERENCES dosen(kd_ds)
      ```
    - Dalam CREATE TABLE:
      ```sql
      CREATE TABLE mahasiswa(
      nim VARCHAR(10) NOT NULL,
      nama VARCHAR(100) NOT NULL,
      kd_ds VARCHAR(10),
      PRIMARY KEY(nim),
      CONSTRAINT fk_DosenWali FOREIGN KEY (kd_ds)
      REFERENCES dosen(kd_ds)
      );
      ```

    ```

    ```

  - Mengubah data
    ```sql
    UPDATE mahasiswa
    SET kd_ds = 'DS011' WHERE nim = 112233445;
    ```
  - Menampilkan CREATE TABLE
    ```sql
    SHOW CREATE TABLE  mahasiswa;
    ```
  - Mode ON UPDATE CASCADE ON DELETE CASCADE
    ```sql
    ALTER TABLE mahasiswa
    DROP FOREIGN KEY fk_mahasiswa_dosen,
    ADD CONSTRAINT fk_dosenwali FOREIGN KEY (kd_ds) REFERENCES dosen(kd_ds) ON UPDATE CASCADE ON DELETE CASCADE;
    ```
  - Menghapus data
    ```sql
    DELETE FROM dosen WHERE kd_ds = 'DS001';
    ```
  - Mode ON UPDATE CASCADE ON DELETE NOT NULL
    ```sql
    ALTER TABLE <table>
    DROP FOREIGN KEY <nama_constraint_lama>,
    ADD CONSTRAINT <nama_constraint_baru> FOREIGN KEY (field) REFERENCES <table_references(filed_references)> ON UPDATE CASCADE ON DELETE NOT NULL;
    ```
  - Mengubah data
    ```sql
    UPDATE dosen
    SET kd_ds = 'DS006' WHERE nama = 'Haha Hihi';
    ```
  - Menghapus data
    ```sql
    DELETE FROM dosen WHERE nim = 'DS003';
      ```

#### Apa bedanya penggunaan RESTRICT dan penggunaan CASCADE
* **Restrict** = yaitu perubahan data dan penghapusan data tidak diijinkan pada tabel referensi (parent table) apabila pada tabel child sudah ada yang merujuk pada data tersebut.
* **Cascade** = yaitu perubahan atau penghapusan data pada tabel referensi (parent table) akan diikuti oleh tabel child.

Keduanya dapat ditambahkan pada action `ON UPDATE` dan `ON DELETE` selain itu, bisa juga digunakan action `SET NULL`.

#### Berikan kesimpulan anda!
Dalam kesimpulannya, **RESTRICT** dan **CASCADE** digunakan untuk mengatur perilaku ketika ada perubahan atau penghapusan data pada tabel utama. Jika digunakan dengan benar, ini dapat membantu memastikan integritas referensial dan konsistensi data antara tabel-tabel yang saling berhubungan.

#### Buat laporan praktikum yang berisi, langkah-langkah praktikum beserta screenshot yang sudah dilakukan dalam bentuk dokumen.
[Link PDF](https://drive.google.com/file/d/1NmJa0T-qU4AtYTB9Hj803KfkaNqm6IKM/view?usp=share_link)

## Finish
