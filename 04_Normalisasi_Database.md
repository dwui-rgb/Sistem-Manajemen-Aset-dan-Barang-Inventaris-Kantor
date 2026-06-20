# 4. Normalisasi Database (UNF → 1NF → 2NF → 3NF)

## A. UNNORMALIZED FORM (UNF)

### Data Awal Tidak Terstruktur

```
TABEL: INVENTARIS_RAW (Data belum dinormalisasi)

┌─────────┬──────────────┬──────────────────┬────────────────┬────────────┬─────────────────┐
│ ID      │ Nama Barang  │ Kategori         │ Lokasi         │ Peminjam   │ Riwayat Kondisi │
├─────────┼──────────────┼──────────────────┼────────────────┼────────────┼─────────────────┤
│ A001    │ Meja Kerja   │ Furniture        │ Lantai 1, R101 │ Budi       │ Baik, 15-Jan    │
│         │              │                  │                │ Ade        │ Baik, 01-Feb    │
│         │              │                  │                │            │ Rusak Ringan,   │
│         │              │                  │                │            │ 10-Mar          │
├─────────┼──────────────┼──────────────────┼────────────────┼────────────┼─────────────────┤
│ A002    │ Kursi Kantor │ Furniture        │ Lantai 2, R202 │ Citra      │ Baik, 20-Jan    │
│         │              │                  │                │ Dino       │ Baik, 05-Feb    │
├─────────┼──────────────┼──────────────────┼────────────────┼────────────┼─────────────────┤
│ A003    │ Printer HP   │ Elektronik       │ Lantai 1, Gudang│ Eka      │ Baik, 01-Jan    │
│         │              │                  │                │            │ Baik Dirawat,   │
│         │              │                  │                │            │ 25-Feb          │
└─────────┴──────────────┴──────────────────┴────────────────┴────────────┴─────────────────┘
```

### Masalah UNF:
- ❌ Repeating groups dalam satu field (Peminjam, Riwayat Kondisi)
- ❌ Multi-valued attributes (banyak nilai dalam satu field)
- ❌ Redundansi data (kategori, lokasi berulang)
- ❌ Update anomaly (kesulitan mengupdate data)
- ❌ Insertion anomaly (tidak bisa insert tanpa duplikasi)
- ❌ Deletion anomaly (kesulitan menghapus data)

---

## B. FIRST NORMAL FORM (1NF)

### Aturan 1NF:
✓ Hapus repeating groups
✓ Setiap field hanya berisi atomic value (nilai tunggal)
✓ Tidak boleh ada multi-valued attributes

### Transformasi ke 1NF:

```
TABEL 1: ASET_1NF
┌─────────┬──────────────┬──────────────┬────────────────────────┐
│ ID      │ Nama_Aset    │ Kategori     │ Lokasi                 │
├─────────┼──────────────┼──────────────┼────────────────────────┤
│ A001    │ Meja Kerja   │ Furniture    │ Lantai 1, R101         │
│ A002    │ Kursi Kantor │ Furniture    │ Lantai 2, R202         │
│ A003    │ Printer HP   │ Elektronik   │ Lantai 1, Gudang       │
└─────────┴──────────────┴──────────────┴────────────────────────┘

TABEL 2: PEMINJAMAN_1NF (Memecah repeating groups)
┌──────────┬──────────┬──────────┬──────────────┐
│ ID_Pinjam│ ID_Aset  │ Peminjam │ Tgl_Pinjam   │
├──────────┼──────────┼──────────┼──────────────┤
│ P001     │ A001     │ Budi     │ 15-Jan-2024  │
│ P002     │ A001     │ Ade      │ 01-Feb-2024  │
│ P003     │ A002     │ Citra    │ 20-Jan-2024  │
│ P004     │ A002     │ Dino     │ 05-Feb-2024  │
│ P005     │ A003     │ Eka      │ 01-Jan-2024  │
└──────────┴──────────┴──────────┴──────────────┘

TABEL 3: KONDISI_RIWAYAT_1NF (Memecah repeating groups)
┌──────────┬──────────┬───────────────┬──────────────┐
│ ID_Riwayat│ ID_Aset │ Kondisi       │ Tgl_Kondisi  │
├──────────┼──────────┼───────────────┼──────────────┤
│ R001     │ A001     │ Baik          │ 15-Jan-2024  │
│ R002     │ A001     │ Baik          │ 01-Feb-2024  │
│ R003     │ A001     │ Rusak Ringan  │ 10-Mar-2024  │
│ R004     │ A002     │ Baik          │ 20-Jan-2024  │
│ R005     │ A002     │ Baik          │ 05-Feb-2024  │
│ R006     │ A003     │ Baik          │ 01-Jan-2024  │
│ R007     │ A003     │ Baik Dirawat  │ 25-Feb-2024  │
└──────────┴──────────┴───────────────┴──────────────┘
```

### Hasil 1NF:
✓ Semua field berisi nilai atomic
✓ Tidak ada repeating groups
✓ Setiap baris independen
✓ Data dapat diinsert tanpa duplikasi

### Masalah 1NF:
- ❌ Partial dependency (Kategori hanya bergantung pada ID Aset, bukan seluruh primary key)
- ❌ Transitive dependency
- ❌ Masih ada redundansi

---

## C. SECOND NORMAL FORM (2NF)

### Aturan 2NF:
✓ Harus memenuhi kriteria 1NF
✓ Hapus partial dependencies
✓ Setiap non-key attribute harus bergantung pada SELURUH primary key
✓ Atribut yang tidak bergantung penuh harus dipisah ke tabel lain

### Transformasi ke 2NF:

```
TABEL 1: ASET_2NF
┌─────────┬──────────────┬──────────────┬────────────────┐
│ ID_Aset │ Nama_Aset    │ ID_Kategori  │ ID_Lokasi      │
├─────────┼──────────────┼──────────────┼────────────────┤
│ A001    │ Meja Kerja   │ K001         │ L001           │
│ A002    │ Kursi Kantor │ K001         │ L002           │
│ A003    │ Printer HP   │ K002         │ L003           │
└─────────┴──────────────┴──────────────┴────────────────┘

TABEL 2: KATEGORI_2NF (Pisahkan atribut kategori)
┌──────────────┬──────────────────┐
│ ID_Kategori  │ Nama_Kategori    │
├──────────────┼──────────────────┤
│ K001         │ Furniture        │
│ K002         │ Elektronik       │
└──────────────┴──────────────────┘

TABEL 3: LOKASI_2NF (Pisahkan atribut lokasi)
┌──────────┬──────────────┬────────┬──────────┐
│ ID_Lokasi│ Nama_Lokasi  │ Lantai │ Ruangan  │
├──────────┼──────────────┼────────┼──────────┤
│ L001     │ Ruang Kerja  │ 1      │ R101     │
│ L002     │ Ruang Kerja  │ 2      │ R202     │
│ L003     │ Gudang Aset  │ 1      │ Gudang   │
└──────────┴──────────────┴────────┴──────────┘

TABEL 4: PEMINJAMAN_2NF
┌──────────┬──────────┬────────────┬──────────────┬──────────────────┐
│ ID_Pinjam│ ID_Aset  │ ID_Pegawai │ Tgl_Pinjam   │ Tgl_Pengembalian  │
├──────────┼──────────┼────────────┼──────────────┼──────────────────┤
│ P001     │ A001     │ PG001      │ 15-Jan-2024  │ 20-Jan-2024      │
│ P002     │ A001     │ PG002      │ 01-Feb-2024  │ 05-Feb-2024      │
│ P003     │ A002     │ PG003      │ 20-Jan-2024  │ 22-Jan-2024      │
│ P004     │ A002     │ PG004      │ 05-Feb-2024  │ 08-Feb-2024      │
│ P005     │ A003     │ PG005      │ 01-Jan-2024  │ NULL             │
└──────────┴──────────┴────────────┴──────────────┴──────────────────┘

TABEL 5: PEGAWAI_2NF
┌────────────┬──────────┬──────────┐
│ ID_Pegawai │ Nama     │ Bagian   │
├────────────┼──────────┼──────────┤
│ PG001      │ Budi     │ IT       │
│ PG002      │ Ade      │ HR       │
│ PG003      │ Citra    │ Ops      │
│ PG004      │ Dino     │ Keuangan │
│ PG005      │ Eka      │ IT       │
└────────────┴──────────┴──────────┘

TABEL 6: RIWAYAT_KONDISI_2NF
┌──────────────┬──────────┬────────────────┬──────────────┐
│ ID_Riwayat   │ ID_Aset  │ Kondisi        │ Tgl_Kondisi  │
├──────────────┼──────────┼────────────────┼──────────────┤
│ R001         │ A001     │ Baik           │ 15-Jan-2024  │
│ R002         │ A001     │ Baik           │ 01-Feb-2024  │
│ R003         │ A001     │ Rusak Ringan   │ 10-Mar-2024  │
│ R004         │ A002     │ Baik           │ 20-Jan-2024  │
│ R005         │ A002     │ Baik           │ 05-Feb-2024  │
│ R006         │ A003     │ Baik           │ 01-Jan-2024  │
│ R007         │ A003     │ Baik Dirawat   │ 25-Feb-2024  │
└──────────────┴──────────┴────────────────┴──────────────┘
```

### Hasil 2NF:
✓ Memenuhi kriteria 1NF
✓ Tidak ada partial dependencies
✓ Setiap non-key attribute bergantung pada seluruh primary key
✓ Redundansi berkurang

### Masalah 2NF:
- ❌ Transitive dependency masih ada
  - Contoh: Kondisi → Poin_Kondisi (Kondisi bukan primary key, tapi Poin_Kondisi bergantung padanya)
  - Contoh: Lantai, Ruangan → Lokasi → ID_Lokasi

---

## D. THIRD NORMAL FORM (3NF)

### Aturan 3NF:
✓ Harus memenuhi kriteria 2NF
✓ Hapus transitive dependencies
✓ Tidak ada non-key attribute yang bergantung pada non-key attribute lain
✓ Setiap non-key attribute hanya bergantung pada primary key

### Transformasi ke 3NF:

```
TABEL 1: ASET_3NF (Final)
┌─────────┬──────────────┬──────────────┬────────────────┬──────────┬────────────────┐
│ ID_Aset │ Kode_Aset    │ Nama_Aset    │ ID_Kategori    │ ID_Lokasi│ ID_Kondisi     │
├─────────┼──────────────┼──────────────┼────────────────┼──────────┼────────────────┤
│ A001    │ AST-2024-001 │ Meja Kerja   │ K001           │ L001     │ KD001          │
│ A002    │ AST-2024-002 │ Kursi Kantor │ K001           │ L002     │ KD001          │
│ A003    │ AST-2024-003 │ Printer HP   │ K002           │ L003     │ KD002          │
└─────────┴──────────────┴──────────────┴────────────────┴──────────┴────────────────┘

TABEL 2: KATEGORI_3NF
┌──────────────┬──────────────────┬──────────────────┐
│ ID_Kategori  │ Nama_Kategori    │ Deskripsi        │
├───��──────────┼──────────────────┼──────────────────┤
│ K001         │ Furniture        │ Perabotan Kantor │
│ K002         │ Elektronik       │ Peralatan Elektr │
└──────────────┴──────────────────┴──────────────────┘

TABEL 3: KONDISI_ASET_3NF (Pisahkan transitive dependency)
┌────────────┬──────────────────┬──────────────┐
│ ID_Kondisi │ Nama_Kondisi     │ Poin_Kondisi │
├────────────┼──────────────────┼──────────────┤
│ KD001      │ Baik             │ 100          │
│ KD002      │ Baik Dirawat     │ 80           │
│ KD003      │ Rusak Ringan     │ 50           │
│ KD004      │ Rusak Berat      │ 20           │
└────────────┴──────────────────┴──────────────┘

TABEL 4: LOKASI_3NF
┌──────────┬──────────────┬────────┬──────────┐
│ ID_Lokasi│ Nama_Lokasi  │ Lantai │ Ruangan  │
├──────────┼──────────────┼────────┼──────────┤
│ L001     │ Ruang Kerja  │ 1      │ R101     │
│ L002     │ Ruang Kerja  │ 2      │ R202     │
│ L003     │ Gudang Aset  │ 1      │ Gudang   │
└──────────┴──────────────┴────────┴──────────┘

TABEL 5: PEGAWAI_3NF
┌────────────┬──────────┬────────┬──────────────┬──────────┬────────────────┐
│ ID_Pegawai │ Nama     │ NIP    │ Bagian       │ Email    │ No_HP          │
├────────────┼──────────┼────────┼──────────────┼──────────┼────────────────┤
│ PG001      │ Budi     │ 20001  │ IT           │ b@mail.c │ 081234567890   │
│ PG002      │ Ade      │ 20002  │ HR           │ a@mail.c │ 082345678901   │
│ PG003      │ Citra    │ 20003  │ Ops          │ c@mail.c │ 083456789012   │
│ PG004      │ Dino     │ 20004  │ Keuangan     │ d@mail.c │ 084567890123   │
│ PG005      │ Eka      │ 20005  │ IT           │ e@mail.c │ 085678901234   │
└────────────┴──────────┴────────┴──────────────┴──────────┴────────────────┘

TABEL 6: PEMINJAMAN_ASET_3NF
┌──────────┬──────────┬────────────┬──────────────┬──────────────────┬─────────┐
│ ID_Pinjam│ ID_Aset  │ ID_Pegawai │ Tgl_Pinjam   │ Tgl_Pengembalian  │ Status  │
├──────────┼──────────┼────────────┼──────────────┼──────────────────┼─────────┤
│ P001     │ A001     │ PG001      │ 15-Jan-2024  │ 20-Jan-2024      │ Selesai │
│ P002     │ A001     │ PG002      │ 01-Feb-2024  │ 05-Feb-2024      │ Selesai │
│ P003     │ A002     │ PG003      │ 20-Jan-2024  │ 22-Jan-2024      │ Selesai │
│ P004     │ A002     │ PG004      │ 05-Feb-2024  │ 08-Feb-2024      │ Selesai │
│ P005     │ A003     │ PG005      │ 01-Jan-2024  │ NULL             │ Aktif   │
└──────────┴──────────┴────────────┴──────────────┴──────────────────┴─────────┘

TABEL 7: RIWAYAT_PERUBAHAN_3NF
┌──────────────┬──────────┬──────────────────┬──────────────┐
│ ID_Riwayat   │ ID_Aset  │ Tgl_Perubahan    │ Keterangan   │
├──────────────┼──────────┼──────────────────┼──────────────┤
│ R001         │ A001     │ 15-Jan-2024      │ Kondisi: Baik│
│ R002         │ A001     │ 01-Feb-2024      │ Kondisi: Baik│
│ R003         │ A001     │ 10-Mar-2024      │ Kondisi: Rusk│
└──────────────┴──────────┴──────────────────┴──────────────┘

TABEL 8: PENGGUNA_3NF
┌──────────────┬──────────┬─────────────┬────────────┬──────┐
│ ID_Pengguna  │ Username │ Password    │ ID_Pegawai │ Role │
├──────────────┼──────────┼─────────────┼────────────┼──────┤
│ 1            │ budi     │ hash...     │ PG001      │ User │
│ 2            │ ade      │ hash...     │ PG002      │ User │
│ 3            │ admin    │ hash...     │ PG003      │ Admin│
└──────────────┴──────────┴─────────────┴────────────┴──────┘
```

### Diagram 3NF:

```
                    SISTEM MANAJEMEN ASET (3NF)

┌──────────────┐    ┌──────────────┐    ┌─────────────────┐
│   KATEGORI   │    │   LOKASI     │    │  KONDISI_ASET   │
├──────────────┤    ├──────────────┤    ├─────────────────┤
│ ID_Kategori  │    │ ID_Lokasi    │    │ ID_Kondisi      │
│ Nama_Kategori│    │ Nama_Lokasi  │    │ Nama_Kondisi    │
│ Deskripsi    │    │ Lantai       │    │ Poin_Kondisi    │
└──────────────┘    │ Ruangan      │    └─────────────────┘
       △            └──────────────┘              △
       │                   △                      │
       │ (1:M)            │ (1:M)            (1:M)│
       │                  │                       │
   ┌───────────────────────┴───────────────────────┐
   │                      ASET                      │
   ├──────────────────────────────────────────────┤
   │ ID_Aset (PK)                                 │
   │ Kode_Aset (UNIQUE)                           │
   │ Nama_Aset                                    │
   │ ID_Kategori (FK) ──────────────→ KATEGORI   │
   │ ID_Lokasi (FK) ─────────────────→ LOKASI    │
   │ ID_Kondisi (FK) ───────────────→ KONDISI    │
   │ Harga_Perolehan                              │
   │ Tgl_Perolehan                                │
   │ Status                                       │
   └──────────────────────┬───────────────────────┘
                         │
          ┌──────────────┴──────────────┐
          │ (1:M)                  (1:M)│
          ▼                             ▼
   ┌──────────────────┐        ┌──────────────────────┐
   │ PEMINJAMAN_ASET  │        │ RIWAYAT_PERUBAHAN    │
   ├──────────────────┤        ├──────────────────────┤
   │ ID_Peminjaman(PK)│        │ ID_Riwayat (PK)      │
   │ ID_Aset (FK)     │        │ ID_Aset (FK)         │
   │ ID_Pegawai (FK)  │        │ Tgl_Perubahan        │
   │ Tgl_Peminjaman   │        │ Keterangan           │
   │ Tgl_Pengembalian │        └──────────────────────┘
   │ Status           │
   └────────┬─────────┘
            │ (M:1)
            ▼
   ┌──────────────────┐
   │    PEGAWAI       │
   ├──────────────────┤
   │ ID_Pegawai (PK)  │
   │ Nama             │
   │ NIP (UNIQUE)     │
   │ Bagian           │
   │ Email            │
   │ No_HP            │
   └────────┬─────────┘
            │ (1:M)
            ▼
   ┌──────────────────┐     ┌──────────────────┐
   │    PENGGUNA      │────→│  LOG_AKTIVITAS   │
   ├──────────────────┤     ├──────────────────┤
   │ ID_Pengguna (PK) │(1:M)│ ID_Log (PK)      │
   │ Username (UNIQUE)│     │ ID_Pengguna (FK) │
   │ Password         │     │ Aktivitas        │
   │ ID_Pegawai (FK)  │     │ Tabel_Target     │
   │ Role             │     │ Tgl_Aktivitas    │
   │ Status           │     │ IP_Address       │
   └──────────────────┘     └──────────────────┘
```

### Hasil 3NF:
✓ Memenuhi kriteria 2NF
✓ Tidak ada transitive dependencies
✓ Setiap atribut hanya bergantung pada primary key
✓ Data terstruktur dengan baik
✓ Redundansi minimal
✓ Query performance optimal

### Keuntungan 3NF:
1. ✓ Konsistensi data terjamin
2. ✓ Mudah untuk maintenance
3. ✓ Query lebih efisien
4. ✓ Update anomaly dihilangkan
5. ✓ Insertion anomaly dihilangkan
6. ✓ Deletion anomaly dihilangkan
7. ✓ Redundansi data minimal
8. ✓ Foreign key relationships jelas

---

## RINGKASAN PROSES NORMALISASI

| Tahap | Kriteria | Status | Masalah |
|-------|----------|--------|----------|
| **UNF** | Repeating groups ada | ❌ Belum valid | Data tidak teratomic |
| **1NF** | Atomic values saja | ✓ Valid | Partial dependency |
| **2NF** | Hapus partial dependency | ✓ Valid | Transitive dependency |
| **3NF** | Hapus transitive dependency | ✓ Valid | Data siap production |

## Final Database Schema (3NF)

**Total Tabel: 11**
- Tabel Master: KATEGORI, LOKASI, KONDISI_ASET, PEGAWAI, PENGGUNA (5 tabel)
- Tabel Transaksi: PEMINJAMAN_ASET, MAINTENANCE (2 tabel)
- Tabel Audit: RIWAYAT_PERUBAHAN, LOG_AKTIVITAS (2 tabel)
- Tabel Aset: ASET, BARANG (2 tabel)

**Total Field: 99**
**Total Relationships: 14**
**Status: SIAP IMPLEMENTASI**
