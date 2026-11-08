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
4. **Data guru**: boleh tambah seorang demi seorang, atau import beramai-ramai (lihat di bawah).
5. Guru boleh terus merekod laporan perjumpaan, program dan pencapaian.

Markah murid dikira daripada markah jawatan yang dipegang dan markah peringkat bagi setiap program selesai yang disertai. Nilai lalai hanyalah cadangan awal, sila laraskan mengikut garis panduan sekolah.

## Rumusan kehadiran

Menu **Rumusan Kehadiran** memaparkan graf kehadiran keseluruhan (cincin peratus, hadir berbanding tidak hadir), perbandingan antara empat komponen (Badan Beruniform, Kelab & Persatuan, Sukan & Permainan, Rumah Sukan), trend mengikut bilangan perjumpaan, taburan kehadiran murid, bar kehadiran bagi setiap unit dalam setiap komponen, dan senarai murid bawah 50%. Penapis komponen, muat turun CSV dan cetakan disediakan. Peratus dikira sebagai jumlah kehadiran dibahagi (bilangan murid × perjumpaan direkod); murid dalam beberapa unit dikira dalam setiap unitnya.

## Import data sekolah (MENU.xlsx)

**Pengurusan & Admin > Setup sistem > Pilih fail MENU.xlsx** memuatkan fail eksport aplikasi rekod kokurikulum sedia ada: unit (`MAKLUMAT UNIT`), murid (`DATA INDUK` dengan butiran `APDM`), guru dan guru penasihat unit (`GURU`), perjumpaan dan kehadiran mingguan (`LAPORAN MINGGUAN`, `KEHADIRAN`, `KEHADIRAN UNIT`), jawatankuasa, program dan keputusan peserta, ekstra kurikulum, serta markah jawatan dan peringkat (`DATA JAWATAN`, `DATA PENCAPAIAN`). Satu paparan pratonton menyenaraikan bilangan rekod dan apa yang dilangkau sebelum anda mengesahkan. Import boleh diulang tanpa membuat rekod berganda. Akaun guru baharu mendapat kata laluan sementara rawak yang dipaparkan sekali.

Fail dibaca dalam pelayar dan hanya ditulis ke pangkalan data eKOKU anda. **Jangan letakkan fail MENU.xlsx atau data murid dalam repo GitHub awam.**

## Daftar guru secara pukal

**Pengurusan & Admin > Data guru > Import guru (Excel / CSV)**.

1. Klik **Templat CSV**, buka dalam Excel dan isi lajur `nama` (wajib), `nokp` (pilihan) dan `peranan` (`guru` atau `admin`; kosong bermaksud guru). Simpan sebagai `.xlsx`, `.xls` atau `.csv`.
2. Pilih fail. Satu paparan pratonton menunjukkan bilangan akaun baharu, akaun yang dikemas kini dan baris yang dilangkau (tiada nama atau pendua).
3. Sahkan. Setiap akaun baharu mendapat kata laluan sementara rawak. Senarai nama, No. KP dan kata laluan dipaparkan **sekali sahaja**, dengan butang untuk memuat turun CSV. Edarkan kepada guru dan padam fail itu.
4. Guru diminta menukar kata laluan semasa log masuk pertama. Akaun yang belum log masuk ditanda "Menunggu log masuk pertama".

Guru sedia ada dipadankan melalui No. KP (atau nama jika tiada No. KP). Kata laluan mereka tidak diubah, dan peranan anda sendiri tidak boleh diturunkan. Fail `.xlsx`/`.xls` memerlukan sambungan internet kerana pustaka pembacanya dimuat dari CDN; fail CSV berfungsi tanpa internet.

## Keselamatan: sila baca sebelum menggunakan data murid sebenar

- Log masuk ialah log masuk peringkat aplikasi. Kata laluan disimpan sebagai cincang SHA-256 berpadanan garam, bukan teks biasa, tetapi cincang itu boleh dibaca oleh sesiapa yang boleh membaca pangkalan data.
- `firestore.rules` hanya menyekat klien yang tidak log masuk. Sesiapa yang membuka halaman eKOKU dan mengetahui config Firebase boleh log masuk secara tanpa nama dan membaca data. Peringkat guru dan pentadbir hanya dikuatkuasakan dalam antara muka, bukan dalam Rules.
- Data murid (nama dan No. KP) ialah data peribadi. Sehingga log masuk ditukar kepada Firebase Authentication dengan Rules bagi setiap peranan, **jangan hoskan eKOKU di URL awam dengan data murid sebenar**. Gunakan data ujian, atau hoskan hanya di rangkaian sekolah.
- Config Firebase boleh dilihat dalam kod halaman. Itu biasa bagi aplikasi web Firebase, tetapi ia menjadikan Rules sebagai satu-satunya pertahanan.

## Model data

Koleksi Firestore: `guru`, `murid`, `unit`, `perjumpaan`, `program`, `jawatan`, `ekstra`, `pencapaian`, dan dokumen tetapan `tetapan/umum`. Keahlian unit disimpan pada setiap murid (`u.BB`, `u.KP`, `u.SP`, `u.RS`). Keputusan program disimpan dalam `program.laporan.hasil`.

## Laporan perjumpaan (borang berlangkah)

Borang 5 langkah supaya mudah diisi (terutama di telefon), mengikut medan laporan mingguan sistem rujukan: **Maklumat** (unit, bil., tarikh, masa, tempat, guru pembimbing: tick guru yang ditetapkan pada unit itu sahaja), **Kehadiran**, **Laporan** (tajuk, huraian, PikeBM, **kemahiran masa hadapan**, kemahiran berfikir + kata kerja KBAT (hanya kata kerja bagi tahap yang dipilih) + gambar KBAT, nilai sivik, refleksi dan impak + gambar laporan murid), **Gambar** (gambar 1-4; dikecilkan automatik) dan **Pengesahan** (disediakan oleh: pilih seorang daripada senarai; disemak oleh Guru Penolong Kanan Kokurikulum; disahkan oleh Guru Besar / Pengetua; semuanya dengan tandatangan jari/tetikus). Langkah tidak boleh dilangkau jika medan wajib kosong. Cetakan memuatkan semua medan, gambar dan tandatangan.

Senarai **Kemahiran Masa Hadapan** diurus oleh admin di Pengurusan & Admin > Setup sistem (tambah, ubah, buang elemen). Senarai awal ialah cadangan; sila laraskan mengikut garis panduan sekolah.

## Komponen dan unit guru

Seorang guru mesti dilantik dalam **sekurang-kurangnya 2 komponen** (tiada had maksimum; boleh semua 4), dan dalam setiap komponen hanya **satu unit**. Tetapkan di Pengurusan & Admin > Data guru (satu dropdown unit bagi setiap komponen). Borang guru ditolak jika kurang 2 komponen. Akaun pentadbir dikecualikan daripada syarat minimum. Di Daftar Unit, sistem hanya menolak unit kedua dalam komponen yang sama.

Import guru pukal (Excel / CSV) menerima lajur unit: `unit beruniform`, `unit kelab & persatuan`, `unit sukan & permainan`, `unit rumah sukan` (nama unit mesti sama dengan yang didaftarkan; huruf besar/kecil tidak penting). Baris yang jumlah komponennya (termasuk unit sedia ada guru itu) kurang daripada 2 **ditolak** dan disenaraikan dalam pratonton; baris lain tetap diimport. Sel kosong tidak mengubah tetapan unit sedia ada. Muat turun templat CSV untuk contoh.

## Logo sekolah dan peratusan keseluruhan

- **Logo sekolah:** muat naik di Pengurusan & Admin > Setup sistem > Profil sekolah (dikecilkan automatik kepada 256px). Dipaparkan pada skrin log masuk, bar sisi dan kepala cetakan laporan.
- **Peratusan keseluruhan kehadiran** (Utama, Rumusan Kehadiran, cetakan dan CSV) hanya mengira 3 komponen: Badan Beruniform, Kelab & Persatuan, dan Sukan & Permainan. Rumah Sukan tidak dikira, tetapi masih dipaparkan sebagai kad komponen sendiri apabila ditapis.
