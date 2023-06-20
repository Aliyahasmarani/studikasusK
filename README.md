# pertemuan13_sems2

```s
NAMA    : ALIYAH ASMARANI
NIM     : 312210203
KELAS   : TI.22.A.2
MATKUL  : BASIS DATA
```

# Membuat Table 
```
CREATE TABLE perusahaan (
    -> id_p VARCHAR (10) PRIMARY KEY,
    -> nama VARCHAR (45),
    -> alamat VARCHAR (45));
```

```
CREATE TABLE departemen (
    -> id_dept VARCHAR (10) PRIMARY KEY,
    -> nama VARCHAR (45),
    -> id_p VARCHAR (10),
    -> manager_nik VARCHAR (10));
```

```
CREATE TABLE karyawan (
    -> nik VARCHAR (10) PRIMARY KEY,
    -> nama VARCHAR (45),
    -> id_dept VARCHAR (10),
    -> sup_nik VARCHAR (10));
```

```
CREATE TABLE project_detail (
    -> id_proj VARCHAR (10),
    -> nik VARCHAR (10));
```

```
CREATE TABLE project (
    ->   id_proj VARCHAR(10) PRIMARY KEY,
    ->   nama VARCHAR(45),
    ->   tgl_mulai DATETIME,
    ->   tgl_selesai DATETIME,
    ->   status TINYINT(1));
```

# Input data
```
INSERT INTO perusahaan (id_p, nama, alamat)
    -> VALUES
    -> ('P01', 'Kantor Pusat', NULL),
    -> ('P02', 'Cabang Bekasi', NULL);
```

```
INSERT INTO departemen (id_dept, nama, id_p, manager_nik)
    -> VALUES
    -> ('D01', 'Produksi', 'P02', 'N01'),
    -> ('D02', 'Marketing', 'P01', 'N03'),
    -> ('D03', 'RnD', 'P02', NULL),
    -> ('D04', 'Logistik', 'P02', NULL);
```

```
INSERT INTO karyawan (nik, nama, id_dept, sup_nik)
    -> VALUES
    -> ('N01', 'Ari', 'D01', NULL),
    -> ('N02', 'Dina', 'D01', NULL),
    -> ('N03', 'Rika', 'D03', NULL),
    -> ('N04', 'Ratih', 'D01', 'N01'),
    -> ('N05', 'Riko', 'D01', 'N01'),
    -> ('N06', 'Dani', 'D02', NULL),
    -> ('N07', 'Anis', 'D02', 'N06'),
    -> ('N08', 'Dika', 'D02', 'N06');
```

```
INSERT INTO project_detail (id_proj, nik)
    -> VALUES
    -> ('PJ01', 'N01'),
    -> ('PJ01', 'N02'),
    -> ('PJ01', 'N03'),
    -> ('PJ01', 'N04'),
    -> ('PJ01', 'N05'),
    -> ('PJ01', 'N06'),
    -> ('PJ01', 'N07'),
    -> ('PJ01', 'N08'),
    -> ('PJ02', 'N01'),
    -> ('PJ02', 'N03'),
    -> ('PJ02', 'N05'),
    -> ('PJ03', 'N03'),
    -> ('PJ03', 'N07'),
    -> ('PJ03', 'N08');
```

```
INSERT INTO project (id_proj, nama, tgl_mulai, tgl_selesai, status)
    -> VALUES
    -> ('PJ01', 'A', '2019-01-10', '2019-03-10', '1'),
    -> ('PJ02', 'B', '2019-02-15', '2019-04-10', '1'),
    -> ('PJ03', 'C', '2019-03-21', '2019-05-10', '1');
```
![2](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/70b7dd14-d7ff-425e-9f29-7ffaa6be759b)
![1](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/38e53790-b920-4d68-9806-732198c09e88)

# Menyambungkan Table

```
ALTER TABLE departemen ADD CONSTRAINT FOREIGN KEY (id_p) REFERENCES perusahaan (id_p);
ALTER TABLE karyawan ADD CONSTRAINT FOREIGN KEY  (id_dept) REFERENCES departemen (id_dept);
ALTER TABLE departemen ADD CONSTRAINT FOREIGN KEY (manager_nik) REFERENCES karyawan (nik);
ALTER TABLE karyawan ADD CONSTRAINT FOREIGN KEY (sup_nik) REFERENCES karyawan (nik);
ALTER TABLE project_detail ADD CONSTRAINT FOREIGN KEY (nik) REFERENCES karyawan (nik);
ALTER TABLE project_detail ADD CONSTRAINT FOREIGN KEY (id_proj) REFERENCES project (id_proj);
```

# Menampilkan nama Manager tiap Departemen

```
SELECT d.nama AS departemen, k.nama AS manager
    -> FROM departemen d
    -> LEFT JOIN karyawan k ON d.manager_nik = k.nik;
```
![3](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/5d7aa0d5-44ea-4e1e-bdd2-81969789312a)

# Menampilkan nama Supervisor tiap karyawan

```
SELECT k.nik, k.nama, d.nama departemen, s.nama supervisor
    -> FROM karyawan k
    -> LEFT JOIN karyawan s ON s.nik = k.sup_nik
    -> LEFT JOIN departemen d ON d.id_dept = k.id_dept;
```
![4](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/ea89e640-e04b-4b95-be55-526b53c4ea8b)

# Menampilkan daftar karyawan yang bekerja pada project A

```
SELECT k.nik, k.nama
    -> FROM karyawan k
    -> JOIN project_detail pj_d ON pj_d.nik = k.nik
    -> JOIN project pj ON pj.id_proj = pj_d.id_proj
    -> WHERE pj.nama = 'A';
```
![5](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/0d4b7065-4188-44fc-a861-40ed21cfe01a)

# LATIHAN PRAKTIKUM
Buat Query untuk menampilkan :

### 1. Departemen apa saja yang terlibat dalam tiap-tiap project?

```
SELECT p.nama AS project, d.nama AS departemen
FROM project p
INNER JOIN project_detail pd ON p.id_proj = pd.id_proj
INNER JOIN karyawan k ON pd.nik = k.nik
INNER JOIN departemen d ON k.id_dept = d.id_dept
ORDER BY p.nama, d.nama;
```
![6](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/7b10fce3-fc72-405c-adca-a7ffeb0b5308)

### 2. Jumlah karyawan tiap departemen yang bekerja pada tiap-tiap project

```
SELECT p.nama AS project, d.nama AS departemen, COUNT(k.nik) AS jumlah_karyawan
FROM project p
INNER JOIN project_detail pd ON p.id_proj = pd.id_proj
INNER JOIN karyawan k ON pd.nik = k.nik
INNER JOIN departemen d ON k.id_dept = d.id_dept
GROUP BY p.nama, d.nama
ORDER BY p.nama, d.nama;
```
![7](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/79bf6e40-9e3d-43b1-9e08-3e8eaa7314d5)

### 3. Ada berapa project yang sedang dikerjakan oleh departemen RnD? (ket:project berjalan adalah yang statusnya 1)!

```
SELECT COUNT(p.id_proj) AS jumlah_proyek
    -> FROM project p
    -> INNER JOIN project_detail pd ON p.id_proj = pd.id_proj
    -> INNER JOIN karyawan k ON pd.nik = k.nik
    -> INNER JOIN departemen d ON k.id_dept = d.id_dept
    -> WHERE d.nama = 'RnD' AND p.status = 1;
```
![8](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/f93d71ce-89d6-4a79-b8b4-82713d4ef4d8)

### 4. Berapa banyak project yang sedang dikerjakan oleh Ari?

```
SELECT COUNT(p.id_proj) AS jumlah_proyek
    -> FROM project p
    -> INNER JOIN project_detail pd ON p.id_proj = pd.id_proj
    -> INNER JOIN karyawan k ON pd.nik = k.nik
    -> WHERE k.nama = 'Ari';
```
![9](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/558c53d8-7113-4b56-bab6-9df96d9e7390)

### 5. Siapa saja yang mengerjakan project B?

```
SELECT k.nama AS nama_karyawan
    -> FROM karyawan k
    -> INNER JOIN project_detail pd ON k.nik = pd.nik
    -> INNER JOIN project p ON pd.id_proj = p.id_proj
    -> WHERE p.nama = 'B';
```
![10](https://github.com/Aliyahasmarani/studikasusK/assets/115197672/730c13a4-6b44-4483-9726-afdae0bcc300)











