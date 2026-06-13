# System Manajemen Aset dan Barang Inventaris Kantor

## 1. Deskripsi Studi Kasus

Sistem Manajemen Aset dan Barang Inventaris Kantor adalah aplikasi berbasis web yang dirancang untuk mengelola seluruh aset dan barang inventaris di lingkungan kantor secara efisien dan terintegrasi. Sistem ini membantu organisasi melacak, mencatat, dan mengelola status barang-barang kantor mulai dari pembelian, penggunaan, pemeliharaan, hingga penghapusan/penjualan.

---

## 2. Latar Belakang dan Tujuan Sistem

### Latar Belakang
- Banyak kantor masih menggunakan sistem manual (Excel/kertas) untuk mencatat aset
- Kesulitan dalam tracking lokasi dan kondisi barang
- Duplikasi data dan kesalahan pencatatan yang sering terjadi
- Sulit menghasilkan laporan inventaris yang akurat
- Tidak ada sistem notifikasi untuk maintenance/perpanjangan aset

### Tujuan Sistem
1. Digitalisasi manajemen aset kantor
2. Meningkatkan akurasi dan keamanan data inventaris
3. Memudahkan pencarian dan tracking lokasi barang
4. Menghasilkan laporan inventaris secara real-time
5. Meningkatkan efisiensi operasional kantor
6. Mengurangi biaya dan kehilangan aset

---

## 3. Identifikasi Aktor

| No | Aktor | Deskripsi | Tanggung Jawab |
|---|---|---|---|
| 1 | **Admin** | Pengelola sistem utama | Kelola user, setting sistem, backup data |
| 2 | **Manajer Aset** | Kepala departemen aset | Validasi transaksi, laporan, pengambilan keputusan |
| 3 | **Staff Inventory** | Operator inventory | Input data, update status barang, scanning |
| 4 | **Kepala Departemen** | Pimpinan departemen | View laporan, monitoring aset departemen |
| 5 | **Karyawan** | User akhir | Request aset, lihat aset yang dipinjam |

---

## 4. Kebutuhan Fungsional (Minimal 10 Kebutuhan)

### A. Manajemen Aset
1. **Tambah Aset Baru** - Input data aset baru dengan detail lengkap (nama, kategori, harga, tanggal perolehan, lokasi)
2. **Edit Aset** - Perubahan data aset yang sudah ada
3. **Hapus Aset** - Menghapus atau mengarsipkan aset yang sudah tidak digunakan
4. **View Detail Aset** - Melihat informasi lengkap satu aset termasuk history
5. **Cari/Filter Aset** - Pencarian berdasarkan nama, kategori, lokasi, atau status

### B. Tracking & Pemeliharaan
6. **Update Status Aset** - Ubah status (aktif, rusak, maintenance, scrap)
7. **Maintenance Schedule** - Jadwal perawatan dan notifikasi maintenance
8. **Transfer Aset** - Memindahkan aset ke departemen/lokasi lain

### C. Laporan & Analisis
9. **Generate Laporan Inventaris** - Laporan lengkap stok barang per departemen/kategori
10. **Laporan Depresiasi Aset** - Perhitungan nilai aset seiring waktu

### D. Keamanan & Administrasi
11. **Manajemen User** - Tambah, edit, hapus user dengan role berbeda
12. **Audit Trail** - Log semua aktivitas perubahan data

---

## 5. Kebutuhan Data

### Entitas Utama Database

#### Tabel: Users
- user_id (PK)
- username
- email
- password (hashed)
- role (admin, manager, staff, karyawan)
- department_id (FK)
- status (aktif, inactive)
- created_at
- updated_at

#### Tabel: Assets
- asset_id (PK)
- asset_code (unique)
- asset_name
- category_id (FK)
- description
- purchase_date
- purchase_price
- location_id (FK)
- department_id (FK)
- status (aktif, rusak, maintenance, scrap)
- condition (baik, sedang, buruk)
- last_maintenance
- created_at
- updated_at

#### Tabel: Categories
- category_id (PK)
- category_name (Furniture, Electronics, Office Supplies, dll)
- description

#### Tabel: Locations
- location_id (PK)
- location_name (Ruang Kepala, Ruang Rapat, Warehouse, dll)
- building
- floor

#### Tabel: Departments
- department_id (PK)
- department_name
- manager_id (FK)

#### Tabel: Asset_Transactions
- transaction_id (PK)
- asset_id (FK)
- transaction_type (purchase, transfer, maintenance, scrap)
- from_department_id (FK)
- to_department_id (FK)
- transaction_date
- notes
- created_by (FK)

#### Tabel: Audit_Log
- log_id (PK)
- user_id (FK)
- action (create, update, delete)
- table_name
- record_id
- old_value
- new_value
- timestamp

---

## 6. Diagram Proses (Use Case & Flowchart)

<img width="1440" height="2960" alt="image" src="https://github.com/user-attachments/assets/c33b30f5-61f3-40b7-8edd-b57ccfa62f78" />
- Berikut alur peminjaman aset dari awal sampai selesai:

 1. Karyawan login ke sistem
 2. Cari aset di katalog (filter kategori/lokasi)
 3. Cek ketersediaan aset — jika tidak tersedia, pilih aset lain dan ulangi dari langkah 2
 4. Isi formulir peminjaman (aset, tujuan, durasi)
 5. Sistem kirim notifikasi ke manajer untuk persetujuan
 6. Manajer review dan setujui — jika ditolak, karyawan menerima notifikasi penolakan dan proses berhenti
 7. Staf logistik lakukan serah terima fisik aset ke karyawan
 8. Status aset diperbarui menjadi "Dipinjam", sistem catat tanggal dan waktu
 9. Karyawan gunakan aset — sistem kirim pengingat otomatis jika mendekati batas waktu pengembalian
 10. Karyawan kembalikan aset, staf periksa kondisi — jika rusak dijadwalkan perbaikan, status aset kembali menjadi "Tersedia"


---

## 7. Pembagian Tugas Anggota

1. Dwiansyah Ananta Tulus 2501020127 (deskripsi kasus, latar belakang beserta tujuan)
2. Raihan Febrian 2501020092 (identifikasi Aktor Dan Kebutuhan Fungsional)
3. Fathar Sabil Abdullah 2501020105 (kebutuhan fungsional Dan Kebutuhan Data)
   
---

## 8. Link Repository GitHub

**Repository:** https://github.com/dwui-rgb/Asset-Inventory-Management
