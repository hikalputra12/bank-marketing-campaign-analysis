# Analisis Kampanye Pemasaran Deposito Bank

Proyek ini menggunakan Python untuk menganalisis pola langganan deposito dalam kampanye telepon sebuah bank di Portugal. Dari **45.211 catatan**, **5.289** berakhir dengan langganan deposito, sehingga tingkat konversi keseluruhan adalah **11,70%**.

**Alat:** Python, pandas, Matplotlib, dan Jupyter Notebook  
**Notebook:** [01_campaign_analysis.ipynb](notebooks/01_campaign_analysis.ipynb)  
**Sumber data:** [UCI Bank Marketing](https://archive.ics.uci.edu/dataset/222/bank+marketing)

## Ask — Pertanyaan Bisnis

Analisis ini membantu tim pemasaran menentukan pola kampanye yang layak diuji lebih lanjut.

- Berapa tingkat konversi kampanye secara keseluruhan?
- Bagaimana tingkat konversi berbeda menurut jenis dan jumlah kontak?
- Bagaimana hasil kampanye sebelumnya berkaitan dengan konversi saat ini?

Dalam proyek ini, **konversi** berarti catatan kampanye berakhir dengan langganan deposito. Tingkat konversi dihitung sebagai **jumlah langganan dibagi jumlah catatan** dalam kelompok yang dianalisis. Penyebutnya bukan jumlah panggilan.

## Prepare — Memahami Data

Analisis menggunakan `bank-full.csv`, yang berisi **45.211 baris dan 17 kolom**. File dibaca dengan pandas menggunakan pemisah titik koma (`;`).

| Kolom | Keterangan |
|---|---|
| `y` | Hasil kampanye saat ini: berlangganan deposito (`yes`) atau tidak (`no`) |
| `contact` | Jenis kontak yang tercatat |
| `campaign` | Jumlah kontak selama kampanye saat ini |
| `previous` | Jumlah kontak sebelum kampanye saat ini |
| `poutcome` | Hasil kampanye sebelumnya |

## Process — Memeriksa dan Mengolah Data

Pemeriksaan tidak menemukan nilai kosong yang dikenali pandas atau baris identik di seluruh kolom. Namun, beberapa kolom menyimpan informasi yang tidak diketahui sebagai kategori teks `unknown`.

| Kolom | Jumlah `unknown` |
|---|---:|
| `job` | 288 |
| `education` | 1.857 |
| `contact` | 13.020 |
| `poutcome` | 36.959 |

Semua catatan dipertahankan. Data mentah disimpan sebagai `df_raw`, sedangkan analisis dilakukan pada salinannya. Kolom `converted` dibuat dengan mengubah `yes` menjadi `1` dan `no` menjadi `0`. Jumlah nilai `1` menunjukkan jumlah konversi, dan rata-ratanya menunjukkan tingkat konversi.

Untuk riwayat kampanye, `previous = 0` digunakan untuk menandai catatan **tanpa kontak sebelumnya**. Catatan yang pernah dihubungi tetapi memiliki `poutcome = "unknown"` dimasukkan ke kelompok terpisah.

## Analyze — Membandingkan Tingkat Konversi

### Berdasarkan jenis kontak

| Jenis kontak | Catatan | Konversi | Tingkat konversi |
|---|---:|---:|---:|
| `cellular` | 29.285 | 4.369 | 14,92% |
| `telephone` | 2.906 | 390 | 13,42% |
| `unknown` | 13.020 | 530 | 4,07% |

Di antara jenis yang diketahui, kontak seluler memiliki tingkat konversi **1,50 poin persentase** lebih tinggi daripada telepon. Kategori `unknown` menunjukkan jenis kontak yang tidak tercatat dan tidak diperlakukan sebagai kanal tersendiri.

### Berdasarkan jumlah kontak

| Jumlah kontak | Catatan | Konversi | Tingkat konversi |
|---|---:|---:|---:|
| 1 | 17.544 | 2.561 | 14,60% |
| 2 | 12.505 | 1.401 | 11,20% |
| 3 | 5.521 | 618 | 11,19% |
| 4–5 | 5.286 | 456 | 8,63% |
| 6+ | 4.355 | 253 | 5,81% |

Kelompok satu kali kontak memiliki tingkat konversi tertinggi. Pola ini belum cukup untuk menetapkan batas panggilan. Nasabah yang langsung setuju mungkin tidak perlu dihubungi lagi, sedangkan nasabah yang belum setuju mungkin menerima panggilan lanjutan.

### Berdasarkan hasil kampanye sebelumnya

| Riwayat kampanye | Catatan | Konversi saat ini | Tingkat konversi saat ini |
|---|---:|---:|---:|
| Berhasil (`success`) | 1.511 | 978 | 64,73% |
| Hasil tidak diketahui, tetapi pernah dihubungi | 5 | 2 | 40,00% |
| Hasil lain (`other`) | 1.840 | 307 | 16,68% |
| Gagal (`failure`) | 4.901 | 618 | 12,61% |
| Tanpa kontak sebelumnya | 36.954 | 3.384 | 9,16% |

Kelompok dengan riwayat keberhasilan memiliki **tingkat konversi tertinggi**. Kelompok tanpa kontak sebelumnya menghasilkan **jumlah konversi terbanyak** karena jumlah catatannya jauh lebih besar. Angka 40,00% pada kelompok dengan hasil yang tidak diketahui hanya berasal dari lima catatan, sehingga tidak dijadikan dasar rekomendasi.

## Share — Temuan Utama

Temuan yang paling menonjol adalah perbedaan tingkat konversi antara kelompok dengan riwayat kampanye berhasil (**64,73%**) dan kelompok tanpa kontak sebelumnya (**9,16%**). Riwayat kampanye bisa digunakan untuk memilih kelompok yang layak diuji. Ukuran kelompok tetap perlu diperhatikan karena persentase tertinggi belum tentu menghasilkan jumlah langganan terbanyak.

[Notebook analisis](notebooks/01_campaign_analysis.ipynb) memuat kode, tabel, dan tiga grafik yang membandingkan konversi menurut jenis kontak, jumlah kontak, dan hasil kampanye sebelumnya. Garis putus-putus pada grafik menunjukkan tingkat konversi keseluruhan **11,70%**.

## Act — Rekomendasi dan Rencana Pengujian

Uji **penawaran yang disesuaikan** pada nasabah yang memenuhi syarat dan memiliki riwayat kampanye berhasil.

1. Alokasikan nasabah secara acak ke kelompok penawaran yang disesuaikan atau penawaran standar.
2. Gunakan aturan kontak dan periode pengamatan yang sama untuk kedua kelompok.
3. Bandingkan proporsi nasabah yang berlangganan dari seluruh nasabah yang dialokasikan ke masing-masing kelompok. Laporkan ukuran kelompok, jumlah langganan, dan selisih konversi dalam poin persentase.

Pengujian ini masih berupa usulan. Analisis historis membantu menentukan kelompok yang layak diuji, tetapi belum mengukur dampak penawaran tersebut.

## Keterbatasan

- Data bersifat observasional. Perbedaan konversi belum membuktikan bahwa mengganti jenis kontak atau mengurangi panggilan akan meningkatkan langganan.
- Sebanyak 13.020 catatan tidak memiliki informasi jenis kontak, sehingga perbandingan antara seluler dan telepon hanya berlaku untuk catatan yang jenis kontaknya diketahui.
- Hasil menggambarkan kampanye dalam dataset historis. Tingkat konversi pada periode atau kampanye lain perlu diukur kembali.

## Cara Menjalankan Proyek

**1. Siapkan data.** Unduh dataset dari [UCI Bank Marketing](https://archive.ics.uci.edu/dataset/222/bank+marketing), ekstrak `bank.zip`, lalu simpan `bank-full.csv` di `data/raw/bank-full.csv` dalam folder proyek.

**2. Siapkan environment Python.** Jalankan perintah berikut di PowerShell dari folder utama repository:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

**3. Jalankan notebook.** Buka `notebooks/01_campaign_analysis.ipynb` di VS Code, pilih kernel Python dari `.venv`, lalu pilih **Run All**. Jika file yang digunakan benar, notebook akan membaca **45.211 baris dan 17 kolom** serta menghasilkan tingkat konversi keseluruhan **11,70%**.

## Referensi

Moro, S., Rita, P., & Cortez, P. (2014). *Bank Marketing* [Dataset]. UCI Machine Learning Repository. [https://doi.org/10.24432/C5K306](https://doi.org/10.24432/C5K306)