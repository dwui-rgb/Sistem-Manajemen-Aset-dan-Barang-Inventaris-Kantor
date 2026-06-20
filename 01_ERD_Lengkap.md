# 1. Entity Relationship Diagram (ERD) Lengkap

## Diagram ERD (Notasi Chen)

```
┌─────────────────────────────────────────────────────────────────────┐
│                    SISTEM MANAJEMEN ASET KANTOR                     │
└─────────────────────────────────────────────────────────────────────┘

                              ENTITAS UTAMA

┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   PEGAWAI    │         │    ASET      │         │   BARANG     │
├──────────────┤         ├──────────────┤         ├──────────────┤
��� id_pegawai(PK)         │ id_aset(PK)  │         │ id_barang(PK)│
│ nama         │         │ kode_aset    │         │ kode_barang  │
│ nip          │         │ nama_aset    │         │ nama_barang  │
│ bagian       │         │ kategori     │         │ kategori     │
│ email        │         │ harga_perolehan        │ harga_satuan │
│ no_hp        │         │ tgl_perolehan          │ stok         │
│ status       │         │ status       │         │ lokasi       │
│ tgl_bergabung           │ lokasi       │         │ status       │
└──────────────┘         │ kondisi      │         │ keterangan   │
       │                 │ keterangan   │         └──────────────┘
       │                 └──────────────┘                │
       │                        │                       │
       │ 1                    1  │  M                 1  │  M
       │─────── MENGGUNAKAN ──────                      │
       │                                                │
       │                 ┌──────────────────┐           │
       │                 │ PEMINJAMAN_ASET  │           │
       │                 ├──────────────────┤           │
       │                 │ id_peminjaman(PK)│           │
       │                 │ id_pegawai(FK)   │           │
       │                 │ id_aset(FK)      │           │
       │                 │ tgl_peminjaman   │           │
       │                 │ tgl_pengembalian │           │
       │                 │ status           │           │
       │                 │ keterangan       │           │
       │                 └──────────────────┘           │
       │                                                │
       └────────────── MEMINJAM ───────────────────────┘

                    TABEL RELASI LAINNYA

┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│  KATEGORI    │         │   LOKASI     │         │ PENGGUNA     │
├──────────────┤         ├──────────────┤         ├──────────────┤
│ id_kategori  │         │ id_lokasi    │         │ id_pengguna  │
│ nama_kategori          │ nama_lokasi  │         │ username     │
│ deskripsi    │         │ lantai       │         │ password     │
└──────────────┘         │ ruangan      │         │ id_pegawai   │
       │                 │ koordinat    │         │ role         │
       │                 └──────────────┘         │ status       │
       │                        │                │ tgl_buat     │
       │ 1                    1  │  M            └──────────────┘
       └─ DIPILIH OLEH ──────────┘                      │
                                                        │
┌──────────────────┐         ┌────────────────────┐    │
│ KONDISI_ASET     │         │ MAINTENANCE        │    │
├──────────────────┤         ├────────────────────┤    │
│ id_kondisi(PK)   │         │ id_maintenance(PK) │    │
│ nama_kondisi     │         │ id_aset(FK)        │    │
│ deskripsi        │         │ tgl_maintenance    │    │
│ poin_kondisi     │         │ jenis_perbaikan    │    │
└──────────────────┘         │ biaya              │    │
       │                     │ status             │    │
       │ 1                   │ catatan            │    │
       └─ MENENTUKAN ────────┴────────────────────┘    │
                                                       │
┌──────────────────────┐    ┌──────────────────────┐   │
│ RIWAYAT_PERUBAHAN    │    │ LOG_AKTIVITAS        │   │
├──────────────────────┤    ├──────────────────────┤   │
│ id_riwayat(PK)       │    │ id_log(PK)           │   │
│ id_aset(FK)          │    │ id_pengguna(FK) ─────┼───┘
│ tgl_perubahan        │    │ aktivitas            │
│ keterangan_perubahan │    │ tabel_target         │
│ nilai_lama           │    │ tgl_aktivitas        │
│ nilai_baru           │    │ ip_address           │
└──────────────────────┘    └──────────────────────┘

```

## Penjelasan Diagram

- **Garis Solid** : Relasi kuat (Strong Relationship)
- **Angka 1** : One (Satu)
- **Angka M** : Many (Banyak)
- **PK** : Primary Key (Kunci Utama)
- **FK** : Foreign Key (Kunci Asing)

## Daftar Entitas

1. **PEGAWAI** - Data karyawan kantor
2. **ASET** - Data aset tetap kantor
3. **BARANG** - Data barang/inventaris
4. **KATEGORI** - Klasifikasi aset/barang
5. **LOKASI** - Tempat penyimpanan aset
6. **KONDISI_ASET** - Status kondisi aset
7. **PEMINJAMAN_ASET** - Transaksi peminjaman
8. **MAINTENANCE** - Riwayat perawatan aset
9. **RIWAYAT_PERUBAHAN** - Audit trail perubahan data
10. **LOG_AKTIVITAS** - Catatan aktivitas pengguna
11. **PENGGUNA** - User aplikasi sistem

---

**Total Entitas: 11**
**Total Relasi: 8**
**Kardinalitas Dominan: One-to-Many (1:M)**
