# eKOKU: Laporan Perjumpaan Kokurikulum

Aplikasi web satu halaman untuk merekod perjumpaan unit kokurikulum, kehadiran murid, program, jawatan, ekstra kurikulum dan pencapaian. Data disimpan dalam Firebase Firestore. Tema hijau zamrud, ada mod gelap, dan boleh digunakan pada telefon.

## Fail

| Fail | Fungsi |
|---|---|
| `index.html` | Seluruh aplikasi |
| `firebase-config.js` | Tempat anda menampal config Firebase |
| `firestore.rules` | Security Rules cadangan |

## Cuba dahulu tanpa Firebase

Buka `index.html` dalam pelayar. Selagi `firebase-config.js` kosong, eKOKU berjalan dalam **mod demo**: data contoh dimuatkan dan disimpan dalam pelayar anda sahaja.

Log masuk pertama: **Pentadbir Sistem** dengan kata laluan `admin123`. Anda akan diminta menukar kata laluan sebelum meneruskan.

## Sambung ke Firebase

1. Di [Firebase Console](https://console.firebase.google.com), cipta projek.
2. **Build > Firestore Database > Create database**.
3. **Build > Authentication > Sign-in method**, aktifkan **Anonymous**. eKOKU menggunakannya supaya `firestore.rules` boleh menutup pangkalan data kepada klien yang tidak melalui aplikasi.
4. **Project settings > Your apps > Web (`</>`)**, daftarkan aplikasi dan salin objek config.
5. Tampal nilainya ke dalam `firebase-config.js`.
6. **Firestore Database > Rules**, tampal isi `firestore.rules` dan **Publish**.
7. Hoskan folder ini (lihat di bawah), buka halaman, dan log masuk sebagai `Pentadbir Sistem`. Rekod pentadbir dicipta secara automatik pada kali pertama.

Jika halaman tersangkut di skrin sambungan, mesej ralat dipaparkan di situ. Punca biasa ialah Anonymous sign-in belum diaktifkan atau Rules menolak akses.

## Hoskan di GitHub Pages

**Settings > Pages**, pilih branch `main` dan folder `/ (root)`. Aplikasi boleh dibuka di `https://<nama-pengguna>.github.io/<nama-repo>/ekoku/`.

## Mula menggunakan

1. **Pengurusan & Admin > Setup sistem**: isi nama sekolah, sesi dan bilangan perjumpaan yang disasarkan bagi setiap unit. Semak markah peringkat dan jawatan.
2. **Daftar unit**: tambah unit bagi empat kategori (Badan Beruniform, Kelab & Persatuan, Sukan & Permainan, Rumah Sukan).
3. **Data murid**: import daripada Excel atau CSV (muat turun templat dahulu). Lajur unit (`bb`, `kp`, `sp`, `rumah`) mengikut nama unit dan boleh dikosongkan. Murid sedia ada dikemas kini melalui No. KP.
4. **Data guru**: tambah guru dengan kata laluan sementara. Mereka menukarnya semasa log masuk pertama.
5. Guru boleh terus merekod laporan perjumpaan, program dan pencapaian.

Markah murid dikira daripada markah jawatan yang dipegang dan markah peringkat bagi setiap program selesai yang disertai. Nilai lalai hanyalah cadangan awal, sila laraskan mengikut garis panduan sekolah.

## Keselamatan: sila baca sebelum menggunakan data murid sebenar

- Log masuk ialah log masuk peringkat aplikasi. Kata laluan disimpan sebagai cincang SHA-256 berpadanan garam, bukan teks biasa, tetapi cincang itu boleh dibaca oleh sesiapa yang boleh membaca pangkalan data.
- `firestore.rules` hanya menyekat klien yang tidak log masuk. Sesiapa yang membuka halaman eKOKU dan mengetahui config Firebase boleh log masuk secara tanpa nama dan membaca data. Peringkat guru dan pentadbir hanya dikuatkuasakan dalam antara muka, bukan dalam Rules.
- Data murid (nama dan No. KP) ialah data peribadi. Sehingga log masuk ditukar kepada Firebase Authentication dengan Rules bagi setiap peranan, **jangan hoskan eKOKU di URL awam dengan data murid sebenar**. Gunakan data ujian, atau hoskan hanya di rangkaian sekolah.
- Config Firebase boleh dilihat dalam kod halaman. Itu biasa bagi aplikasi web Firebase, tetapi ia menjadikan Rules sebagai satu-satunya pertahanan.

## Model data

Koleksi Firestore: `guru`, `murid`, `unit`, `perjumpaan`, `program`, `jawatan`, `ekstra`, `pencapaian`, dan dokumen tetapan `tetapan/umum`. Keahlian unit disimpan pada setiap murid (`u.BB`, `u.KP`, `u.SP`, `u.RS`). Keputusan program disimpan dalam `program.laporan.hasil`.
