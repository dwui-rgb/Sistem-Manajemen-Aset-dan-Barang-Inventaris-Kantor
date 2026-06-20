# 3. Kamus Data (Data Dictionary)

## A. TABEL PEGAWAI

```
Nama Tabel    : PEGAWAI
Deskripsi     : Menyimpan data karyawan/pegawai kantor
Volume Data   : ~500 baris
Update Rate   : Jarang (hanya saat hire/resign)
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_pegawai | INT | - | N | PK | AUTO_INCREMENT | Identitas unik pegawai |
| 2 | nama | VARCHAR | 100 | N | - | - | Nama lengkap pegawai |
| 3 | nip | VARCHAR | 20 | N | UQ | - | Nomor Induk Pegawai (unik) |
| 4 | bagian | VARCHAR | 50 | N | - | - | Departemen/Bagian kerja |
| 5 | email | VARCHAR | 100 | N | UQ | - | Email kantor pegawai |
| 6 | no_hp | VARCHAR | 15 | Y | - | - | Nomor telepon |
| 7 | status | ENUM | - | N | - | 'Aktif' | 'Aktif', 'Tidak Aktif', 'Cuti' |
| 8 | tgl_bergabung | DATE | - | N | - | - | Tanggal mulai bekerja |
| 9 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 10 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

---

## B. TABEL ASET

```
Nama Tabel    : ASET
Deskripsi     : Menyimpan data aset tetap kantor
Volume Data   : ~1000 baris
Update Rate   : Sering (kondisi, lokasi, status)
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_aset | INT | - | N | PK | AUTO_INCREMENT | Identitas unik aset |
| 2 | kode_aset | VARCHAR | 50 | N | UQ | - | Kode inventaris (unik) |
| 3 | nama_aset | VARCHAR | 100 | N | - | - | Nama/jenis aset |
| 4 | id_kategori | INT | - | N | FK | - | Referensi KATEGORI |
| 5 | harga_perolehan | DECIMAL | 15,2 | N | - | 0 | Harga pembelian aset |
| 6 | tgl_perolehan | DATE | - | N | - | - | Tanggal pembelian |
| 7 | status | ENUM | - | N | - | 'Tersedia' | 'Tersedia','Dipinjam','Rusak','Terjual' |
| 8 | id_lokasi | INT | - | Y | FK | - | Referensi LOKASI |
| 9 | id_kondisi | INT | - | Y | FK | - | Referensi KONDISI_ASET |
| 10 | keterangan | TEXT | - | Y | - | - | Catatan tambahan |
| 11 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 12 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

---

## C. TABEL BARANG

```
Nama Tabel    : BARANG
Deskripsi     : Menyimpan data barang/inventaris kantor
Volume Data   : ~500 baris
Update Rate   : Sering (stok, lokasi)
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_barang | INT | - | N | PK | AUTO_INCREMENT | Identitas unik barang |
| 2 | kode_barang | VARCHAR | 50 | N | UQ | - | Kode inventaris (unik) |
| 3 | nama_barang | VARCHAR | 100 | N | - | - | Nama barang |
| 4 | id_kategori | INT | - | N | FK | - | Referensi KATEGORI |
| 5 | harga_satuan | DECIMAL | 10,2 | N | - | 0 | Harga per unit |
| 6 | stok | INT | - | N | - | 0 | Jumlah barang dalam stok |
| 7 | id_lokasi | INT | - | Y | FK | - | Referensi LOKASI |
| 8 | status | ENUM | - | N | - | 'Tersedia' | 'Tersedia', 'Habis', 'Hilang' |
| 9 | keterangan | TEXT | - | Y | - | - | Catatan tambahan |
| 10 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 11 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

---

## D. TABEL KATEGORI

```
Nama Tabel    : KATEGORI
Deskripsi     : Klasifikasi untuk aset dan barang
Volume Data   : ~20 baris
Update Rate   : Jarang
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_kategori | INT | - | N | PK | AUTO_INCREMENT | Identitas unik kategori |
| 2 | nama_kategori | VARCHAR | 50 | N | UQ | - | Nama kategori (unik) |
| 3 | deskripsi | TEXT | - | Y | - | - | Penjelasan kategori |
| 4 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 5 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

**Contoh Data**:
- Peralatan Kantor
- Furniture
- Kendaraan
- Elektronik
- Alat Bantu

---

## E. TABEL LOKASI

```
Nama Tabel    : LOKASI
Deskripsi     : Tempat penyimpanan aset dan barang
Volume Data   : ~50 baris
Update Rate   : Jarang
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_lokasi | INT | - | N | PK | AUTO_INCREMENT | Identitas unik lokasi |
| 2 | nama_lokasi | VARCHAR | 100 | N | - | - | Nama tempat penyimpanan |
| 3 | lantai | INT | - | N | - | 1 | Nomor lantai |
| 4 | ruangan | VARCHAR | 50 | N | - | - | Nomor/nama ruangan |
| 5 | koordinat | VARCHAR | 100 | Y | - | - | Koordinat GPS (opsional) |
| 6 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 7 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

**Contoh Data**:
- Ruang Rapat Utama (Lantai 2, R201)
- Gudang Barang (Lantai 1, G01)
- Kantor Manager (Lantai 3, R301)

---

## F. TABEL KONDISI_ASET

```
Nama Tabel    : KONDISI_ASET
Deskripsi     : Menentukan kondisi fisik aset
Volume Data   : ~5 baris (statis)
Update Rate   : Tidak pernah
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_kondisi | INT | - | N | PK | - | Identitas unik kondisi |
| 2 | nama_kondisi | VARCHAR | 50 | N | UQ | - | Nama kondisi (unik) |
| 3 | deskripsi | TEXT | - | Y | - | - | Penjelasan kondisi |
| 4 | poin_kondisi | INT | - | N | - | 100 | Nilai poin (0-100) |

**Data Standar**:
| Kondisi | Poin | Deskripsi |
|---------|------|----------|
| Baik | 100 | Aset dalam kondisi prima, tidak ada kerusakan |
| Baik Dirawat | 80 | Aset masih berfungsi, ada sedikit keausan |
| Rusak Ringan | 50 | Aset masih berfungsi tapi ada kerusakan |
| Rusak Berat | 20 | Aset sudah tidak berfungsi dengan baik |
| Tidak Bisa Digunakan | 0 | Aset sudah tidak bisa digunakan sama sekali |

---

## G. TABEL PEMINJAMAN_ASET

```
Nama Tabel    : PEMINJAMAN_ASET
Deskripsi     : Mencatat transaksi peminjaman aset
Volume Data   : ~5000 baris (bertambah setiap hari)
Update Rate   : Sangat sering (realtime)
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_peminjaman | INT | - | N | PK | AUTO_INCREMENT | Identitas unik transaksi |
| 2 | id_pegawai | INT | - | N | FK | - | Referensi PEGAWAI |
| 3 | id_aset | INT | - | N | FK | - | Referensi ASET |
| 4 | tgl_peminjaman | DATETIME | - | N | - | CURRENT_TIMESTAMP | Tanggal/jam peminjaman |
| 5 | tgl_pengembalian | DATETIME | - | Y | - | NULL | Tanggal/jam pengembalian |
| 6 | status | ENUM | - | N | - | 'Aktif' | 'Aktif', 'Dikembalikan', 'Hilang' |
| 7 | keterangan | TEXT | - | Y | - | - | Catatan peminjaman |
| 8 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 9 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

**Index**: (id_pegawai, id_aset, status, tgl_peminjaman)

---

## H. TABEL MAINTENANCE

```
Nama Tabel    : MAINTENANCE
Deskripsi     : Riwayat perawatan dan perbaikan aset
Volume Data   : ~2000 baris
Update Rate   : Sering
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_maintenance | INT | - | N | PK | AUTO_INCREMENT | Identitas unik |
| 2 | id_aset | INT | - | N | FK | - | Referensi ASET |
| 3 | tgl_maintenance | DATE | - | N | - | - | Tanggal maintenance |
| 4 | jenis_perbaikan | VARCHAR | 100 | N | - | - | Jenis perawatan/perbaikan |
| 5 | biaya | DECIMAL | 10,2 | N | - | 0 | Biaya perawatan |
| 6 | status | ENUM | - | N | - | 'Direncanakan' | 'Direncanakan','Sedang Berlangsung','Selesai' |
| 7 | catatan | TEXT | - | Y | - | - | Catatan detail |
| 8 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 9 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

---

## I. TABEL RIWAYAT_PERUBAHAN

```
Nama Tabel    : RIWAYAT_PERUBAHAN
Deskripsi     : Audit trail perubahan data aset (Log)
Volume Data   : ~10000 baris (bertambah setiap hari)
Update Rate   : Otomatis (trigger)
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_riwayat | INT | - | N | PK | AUTO_INCREMENT | Identitas unik |
| 2 | id_aset | INT | - | N | FK | - | Referensi ASET |
| 3 | tgl_perubahan | DATETIME | - | N | - | CURRENT_TIMESTAMP | Waktu perubahan |
| 4 | keterangan_perubahan | VARCHAR | 200 | N | - | - | Deskripsi perubahan |
| 5 | nilai_lama | TEXT | - | Y | - | - | Nilai sebelum perubahan |
| 6 | nilai_baru | TEXT | - | Y | - | - | Nilai sesudah perubahan |

**Digunakan oleh**: Trigger ketika ada UPDATE pada tabel ASET

---

## J. TABEL LOG_AKTIVITAS

```
Nama Tabel    : LOG_AKTIVITAS
Deskripsi     : Catatan semua aktivitas pengguna (Audit Log)
Volume Data   : ~50000 baris
Update Rate   : Otomatis (trigger/event handler)
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_log | INT | - | N | PK | AUTO_INCREMENT | Identitas unik log |
| 2 | id_pengguna | INT | - | N | FK | - | Referensi PENGGUNA |
| 3 | aktivitas | VARCHAR | 200 | N | - | - | Deskripsi aktivitas |
| 4 | tabel_target | VARCHAR | 50 | N | - | - | Tabel yang diakses |
| 5 | tgl_aktivitas | DATETIME | - | N | - | CURRENT_TIMESTAMP | Waktu aktivitas |
| 6 | ip_address | VARCHAR | 45 | Y | - | - | IP address pengguna |
| 7 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |

**Index**: (id_pengguna, tgl_aktivitas, tabel_target)

---

## K. TABEL PENGGUNA

```
Nama Tabel    : PENGGUNA
Deskripsi     : Akun pengguna sistem
Volume Data   : ~50 baris
Update Rate   : Jarang
```

| No | Nama Field | Tipe Data | Size | NULL | Key | Default | Deskripsi |
|-----|-----------|-----------|------|------|-----|---------|----------|
| 1 | id_pengguna | INT | - | N | PK | AUTO_INCREMENT | Identitas unik pengguna |
| 2 | username | VARCHAR | 50 | N | UQ | - | Username login (unik) |
| 3 | password | VARCHAR | 255 | N | - | - | Password (hashed/bcrypt) |
| 4 | id_pegawai | INT | - | N | FK | - | Referensi PEGAWAI |
| 5 | role | ENUM | - | N | - | 'User' | 'Admin', 'Manager', 'User' |
| 6 | status | ENUM | - | N | - | 'Aktif' | 'Aktif', 'Tidak Aktif' |
| 7 | last_login | DATETIME | - | Y | - | NULL | Waktu login terakhir |
| 8 | tgl_buat | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Tanggal pembuatan akun |
| 9 | created_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record dibuat |
| 10 | updated_at | TIMESTAMP | - | N | - | CURRENT_TIMESTAMP | Waktu record diupdate |

---

## RINGKASAN

| Tabel | Jumlah Field | PK | FK | Update Rate | Volume |
|-------|-------|------|----|-----------|---------|
| PEGAWAI | 10 | 1 | 0 | Jarang | ~500 |
| ASET | 12 | 1 | 3 | Sering | ~1000 |
| BARANG | 11 | 1 | 2 | Sering | ~500 |
| KATEGORI | 5 | 1 | 0 | Jarang | ~20 |
| LOKASI | 7 | 1 | 0 | Jarang | ~50 |
| KONDISI_ASET | 4 | 1 | 0 | Tidak Pernah | ~5 |
| PEMINJAMAN_ASET | 9 | 1 | 2 | Sangat Sering | ~5000 |
| MAINTENANCE | 9 | 1 | 1 | Sering | ~2000 |
| RIWAYAT_PERUBAHAN | 6 | 1 | 1 | Otomatis | ~10000 |
| LOG_AKTIVITAS | 7 | 1 | 1 | Otomatis | ~50000 |
| PENGGUNA | 10 | 1 | 1 | Jarang | ~50 |
| **TOTAL** | **99** | **11** | **14** | - | ~70000 |
