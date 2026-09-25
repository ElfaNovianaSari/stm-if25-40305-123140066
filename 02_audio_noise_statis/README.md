# Tugas 2 — Analisis Sinyal Suara & Noise Statis

Eksplorasi representasi audio 4 dimensi (waveform, spektrum FFT, spektrogram STFT, Mel-spektrogram) dan pembuktian efek **aliasing** pada resampling, menggunakan rekaman suara sendiri yang dibacakan di depan sumber derau statis.

**Mata Kuliah:** IF25-40305 Sistem Teknologi Multimedia
**Nama:** Elfa Noviana Sari · **NIM:** 123140066

---

## Perangkat & Sumber Noise

| Item | Keterangan |
|---|---|
| Perangkat perekam | Perekam suara dengan ponsel |
| Sumber derau statis | Kipas angin, putaran konstan |
| Jarak perekam ke sumber | ± 0.5 – 1 meter |
| Format berkas | `.wav` PCM 16-bit, mono |
| Laju sampel asli | 48.000 Hz |
| Durasi | 11,97 detik (574.400 sampel) |
| Materi bacaan | 2–3 kalimat artikel berita |

Derau kipas dipilih karena bersifat **statis (stationary)** — energi dan sebaran frekuensinya relatif konstan dari detik ke detik, sehingga tampak sebagai pita horizontal rata pada spektrogram.

---

## Berkas dalam Folder

| Berkas | Isi |
|---|---|
| `tugas_audio_noise_statis.ipynb` | Notebook lengkap, seluruh sel sudah dieksekusi |
| `tugas_audio_noise_statis.pdf` | Ekspor PDF notebook (cadangan bila GitHub gagal render) |
| `audio_original.wav` | Rekaman asli, 48.000 Hz mono, 16-bit PCM |
| `audio_downsampled_naive_8k.wav` | Naive `x[::6]` → 8.000 Hz, **tanpa** filter anti-aliasing |
| `audio_downsampled_clean_8k.wav` | `resample_poly` → 8.000 Hz, **dengan** filter anti-aliasing |
| `audio_downsampled_naive_11025.wav` | Naive interpolasi linear → 11.025 Hz, tanpa filter |
| `audio_downsampled_clean_11025.wav` | `resample_poly` (147/640) → 11.025 Hz, dengan filter |

---

## Alur Notebook

| Bagian | Isi |
|:---:|---|
| 1 | Import library & pengaturan tampilan plot |
| 2 | Akuisisi audio + ekstraksi metadata (`sr=None`, durasi, jumlah sampel, min/max, RMS) |
| 3 | **Waveform** — deteksi otomatis segmen paling sepi (noise floor) vs paling ramai (vokal aktif) via RMS per blok 0,1 detik |
| 4 | **Spektrum FFT (dBFS)** — window Hann, ternormalisasi bobot window, dengan pembagian zona frekuensi |
| 5 | **Spektrogram STFT** + trade-off resolusi 3 skema jendela (256 / 1024 / 4096) |
| 6 | **Mel-spektrogram** — skala perseptual, formant vokal lebih padat di frekuensi rendah |
| 7 | Galeri gabungan 2×2: satu rekaman, empat sudut pandang |
| 8 | Pembuktian aliasing pada **sinyal sintetis terkontrol** (48 kHz → 8 kHz dan → 11,025 kHz) |
| 9 | Penerapan proses yang sama pada **rekaman asli** |

---

## Ringkasan Temuan

- **Frekuensi dominan derau kipas:** ≈ 187,5 Hz (zona Bass). Energi derau terpusat pada frekuensi rendah–menengah (Sub-Bass & Bass), dan menurun bertahap ke arah Treble/Air.
- **Pemisahan noise vs vokal:** pita energi di bawah 300 Hz tetap muncul saat hening maupun saat berbicara → penanda derau statis. Di atas 300 Hz, pola energi mengikuti aktivitas bicara.
- **Trade-off resolusi STFT:** jendela pendek → waktu presisi, frekuensi kabur (Δf ≈ 188 Hz); jendela panjang → sebaliknya. Jendela 1024 dipakai sebagai kompromi.
- **Aliasing (sinyal uji):** komponen 6.500 Hz pada target 8 kHz terlipat menjadi nada palsu 1.500 Hz (`f_alias = 8000 − 6500`). Hasil serupa pada target 11.025 kHz: 7.000 Hz → 4.025 Hz.
- **Aliasing (rekaman asli):** efeknya halus karena >99% energi rekaman berada di bawah 4 kHz. Panel naive tetap memperlihatkan tambahan energi frekuensi rendah yang tidak muncul pada `resample_poly`.
- **Kesimpulan:** filter anti-aliasing wajib dipakai sebelum penurunan laju sampel. Besar risiko aliasing ditentukan oleh ada-tidaknya filter **dan** seberapa jauh laju sampel diturunkan.

---

## Menjalankan Notebook

Environment & dependensi disiapkan dari root repositori (`../README.md`). Setelah `.venv` aktif:

```bash
jupyter lab tugas_audio_noise_statis.ipynb
```

Pilih kernel **Python (audio-noise)**. Notebook menulis ulang berkas `.wav` di folder ini saat dijalankan, jadi jalankan dari dalam direktori `02_audio_noise_statis/`.

> **File dalam bentuk PDF :** berkas PDF diunggah dengan nama `audio_noise_statis.pdf`.
