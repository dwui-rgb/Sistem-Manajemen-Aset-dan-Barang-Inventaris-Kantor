# 2. Penjelasan Entitas dan Relasi

## A. PENJELASAN ENTITAS

### 1. PEGAWAI
**Fungsi**: Menyimpan data karyawan kantor

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_pegawai | INT (PK) | Identitas unik pegawai (Auto Increment) |
| nama | VARCHAR(100) | Nama lengkap pegawai |
| nip | VARCHAR(20) (UNIQUE) | Nomor Induk Pegawai |
| bagian | VARCHAR(50) | Departemen/Bagian tempat bekerja |
| email | VARCHAR(100) | Email pegawai |
| no_hp | VARCHAR(15) | Nomor telepon pegawai |
| status | ENUM | Status: 'Aktif', 'Tidak Aktif', 'Cuti' |
| tgl_bergabung | DATE | Tanggal bergabung di kantor |

**Constraint**: Tidak boleh ada duplikat NIP, Email harus valid

---

### 2. ASET
**Fungsi**: Menyimpan data aset tetap kantor (Gedung, Kendaraan, Peralatan besar)

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_aset | INT (PK) | Identitas unik aset (Auto Increment) |
| kode_aset | VARCHAR(50) (UNIQUE) | Kode inventaris aset |
| nama_aset | VARCHAR(100) | Nama/jenis aset |
| kategori | INT (FK) | Referensi ke tabel KATEGORI |
| harga_perolehan | DECIMAL(15,2) | Harga pembelian aset |
| tgl_perolehan | DATE | Tanggal pembelian aset |
| status | ENUM | 'Tersedia', 'Dipinjam', 'Rusak', 'Terjual' |
| lokasi | INT (FK) | Referensi ke tabel LOKASI |
| kondisi | INT (FK) | Referensi ke tabel KONDISI_ASET |
| keterangan | TEXT | Catatan tambahan tentang aset |

**Constraint**: Kode aset unik, harga perolehan > 0

---

### 3. BARANG
**Fungsi**: Menyimpan data barang/inventaris konsumtif kantor

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_barang | INT (PK) | Identitas unik barang (Auto Increment) |
| kode_barang | VARCHAR(50) (UNIQUE) | Kode inventaris barang |
| nama_barang | VARCHAR(100) | Nama barang |
| kategori | INT (FK) | Referensi ke tabel KATEGORI |
| harga_satuan | DECIMAL(10,2) | Harga per unit |
| stok | INT | Jumlah barang dalam stok |
| lokasi | INT (FK) | Referensi ke tabel LOKASI |
| status | ENUM | 'Tersedia', 'Habis', 'Hilang' |
| keterangan | TEXT | Catatan tambahan |

**Constraint**: Stok >= 0, harga satuan > 0

---

### 4. KATEGORI
**Fungsi**: Klasifikasi untuk aset dan barang

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_kategori | INT (PK) | Identitas unik kategori |
| nama_kategori | VARCHAR(50) (UNIQUE) | Nama kategori (Peralatan, Furniture, Kendaraan, dll) |
| deskripsi | TEXT | Penjelasan kategori |

---

### 5. LOKASI
**Fungsi**: Menyimpan informasi tempat penyimpanan aset dan barang

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_lokasi | INT (PK) | Identitas unik lokasi |
| nama_lokasi | VARCHAR(100) | Nama tempat (Ruang Rapat, Gudang, etc) |
| lantai | INT | Nomor lantai |
| ruangan | VARCHAR(50) | Nomor/nama ruangan |
| koordinat | VARCHAR(100) | Koordinat GPS (opsional) |

---

### 6. KONDISI_ASET
**Fungsi**: Menentukan kondisi fisik aset

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_kondisi | INT (PK) | Identitas unik kondisi |
| nama_kondisi | VARCHAR(50) (UNIQUE) | 'Baik', 'Baik Dirawat', 'Rusak Ringan', 'Rusak Berat' |
| deskripsi | TEXT | Penjelasan kondisi |
| poin_kondisi | INT | Nilai poin (100 = Baik, 50 = Rusak Ringan, 0 = Rusak Berat) |

---

### 7. PEMINJAMAN_ASET
**Fungsi**: Mencatat transaksi peminjaman aset oleh pegawai

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_peminjaman | INT (PK) | Identitas unik transaksi |
| id_pegawai | INT (FK) | Referensi ke PEGAWAI yang meminjam |
| id_aset | INT (FK) | Referensi ke ASET yang dipinjam |
| tgl_peminjaman | DATETIME | Tanggal dan jam peminjaman |
| tgl_pengembalian | DATETIME | Tanggal dan jam pengembalian (NULL jika belum dikembalikan) |
| status | ENUM | 'Aktif', 'Dikembalikan', 'Hilang' |
| keterangan | TEXT | Catatan peminjaman |

**Constraint**: tgl_pengembalian >= tgl_peminjaman (jika tidak NULL)

---

### 8. MAINTENANCE
**Fungsi**: Mencatat riwayat perawatan dan perbaikan aset

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_maintenance | INT (PK) | Identitas unik |
| id_aset | INT (FK) | Referensi ke ASET yang dirawat |
| tgl_maintenance | DATE | Tanggal perawatan |
| jenis_perbaikan | VARCHAR(100) | Jenis perawatan/perbaikan |
| biaya | DECIMAL(10,2) | Biaya perawatan |
| status | ENUM | 'Direncanakan', 'Sedang Berlangsung', 'Selesai' |
| catatan | TEXT | Catatan detail perbaikan |

---

### 9. RIWAYAT_PERUBAHAN
**Fungsi**: Audit trail untuk melacak perubahan data aset

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_riwayat | INT (PK) | Identitas unik |
| id_aset | INT (FK) | Referensi ke ASET yang diubah |
| tgl_perubahan | DATETIME | Waktu perubahan |
| keterangan_perubahan | VARCHAR(200) | Deskripsi perubahan |
| nilai_lama | TEXT | Nilai sebelum perubahan |
| nilai_baru | TEXT | Nilai sesudah perubahan |

---

### 10. LOG_AKTIVITAS
**Fungsi**: Mencatat semua aktivitas pengguna dalam sistem

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_log | INT (PK) | Identitas unik log |
| id_pengguna | INT (FK) | Referensi ke PENGGUNA yang melakukan aktivitas |
| aktivitas | VARCHAR(200) | Deskripsi aktivitas (Tambah, Ubah, Hapus, Login, etc) |
| tabel_target | VARCHAR(50) | Tabel yang diakses (ASET, BARANG, etc) |
| tgl_aktivitas | DATETIME | Waktu aktivitas |
| ip_address | VARCHAR(45) | IP address pengguna |

---

### 11. PENGGUNA
**Fungsi**: Menyimpan akun pengguna sistem

| Atribut | Tipe | Keterangan |
|---------|------|------------|
| id_pengguna | INT (PK) | Identitas unik pengguna |
| username | VARCHAR(50) (UNIQUE) | Username login |
| password | VARCHAR(255) | Password terenkripsi |
| id_pegawai | INT (FK) | Referensi ke PEGAWAI |
| role | ENUM | 'Admin', 'Manager', 'User' |
| status | ENUM | 'Aktif', 'Tidak Aktif' |
| tgl_buat | DATETIME | Tanggal pembuatan akun |

---

## B. PENJELASAN RELASI

### Relasi 1: PEGAWAI - PEMINJAMAN_ASET (1:M)
**Tipe**: One-to-Many
- Satu pegawai dapat meminjam banyak aset
- Tidak semua pegawai harus meminjam aset
- **Constraint**: id_pegawai di PEMINJAMAN_ASET adalah Foreign Key

### Relasi 2: ASET - PEMINJAMAN_ASET (1:M)
**Tipe**: One-to-Many
- Satu aset dapat dipinjam berkali-kali oleh pegawai berbeda
- Tidak semua aset harus dipinjam
- **Constraint**: id_aset di PEMINJAMAN_ASET adalah Foreign Key

### Relasi 3: BARANG - KATEGORI (M:1)
**Tipe**: Many-to-One
- Banyak barang dapat memiliki kategori yang sama
- Setiap barang harus memiliki satu kategori
- **Constraint**: kategori di BARANG adalah Foreign Key

### Relasi 4: ASET - KATEGORI (M:1)
**Tipe**: Many-to-One
- Banyak aset dapat memiliki kategori yang sama
- Setiap aset harus memiliki satu kategori
- **Constraint**: kategori di ASET adalah Foreign Key

### Relasi 5: ASET/BARANG - LOKASI (M:1)
**Tipe**: Many-to-One
- Banyak aset/barang dapat disimpan di lokasi yang sama
- Setiap aset/barang harus memiliki satu lokasi
- **Constraint**: lokasi di ASET dan BARANG adalah Foreign Key

### Relasi 6: ASET - KONDISI_ASET (M:1)
**Tipe**: Many-to-One
- Banyak aset dapat memiliki kondisi yang sama
- Setiap aset harus memiliki satu kondisi
- **Constraint**: kondisi di ASET adalah Foreign Key

### Relasi 7: ASET - MAINTENANCE (1:M)
**Tipe**: One-to-Many
- Satu aset dapat memiliki banyak riwayat maintenance
- Tidak semua aset harus di-maintenance
- **Constraint**: id_aset di MAINTENANCE adalah Foreign Key

### Relasi 8: ASET - RIWAYAT_PERUBAHAN (1:M)
**Tipe**: One-to-Many
- Satu aset dapat memiliki banyak perubahan data
- Tidak semua aset harus mengalami perubahan
- **Constraint**: id_aset di RIWAYAT_PERUBAHAN adalah Foreign Key

### Relasi 9: PENGGUNA - LOG_AKTIVITAS (1:M)
**Tipe**: One-to-Many
- Satu pengguna dapat melakukan banyak aktivitas
- Setiap log aktivitas harus dicatat dari satu pengguna
- **Constraint**: id_pengguna di LOG_AKTIVITAS adalah Foreign Key

---

## C. RINGKASAN RELASI

| Dari | Ke | Jenis | Kardinalitas |
|------|----|-----------|-----------|
| PEGAWAI | PEMINJAMAN_ASET | Relationship | 1:M |
| ASET | PEMINJAMAN_ASET | Relationship | 1:M |
| ASET | KATEGORI | Attribute | M:1 |
| BARANG | KATEGORI | Attribute | M:1 |
| ASET | LOKASI | Attribute | M:1 |
| BARANG | LOKASI | Attribute | M:1 |
| ASET | KONDISI_ASET | Attribute | M:1 |
| ASET | MAINTENANCE | Relationship | 1:M |
| ASET | RIWAYAT_PERUBAHAN | Relationship | 1:M |
| PENGGUNA | LOG_AKTIVITAS | Relationship | 1:M |
| PENGGUNA | PEGAWAI | Attribute | 1:1 |

**Total: 11 Relasi**