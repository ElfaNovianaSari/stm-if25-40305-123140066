# STM IF25-40305 

Repositori tugas mata kuliah **IF25-40305 Sistem Teknologi Multimedia**.

| | |
|---|---|
| **Nama** | Elfa Noviana Sari |
| **NIM** | 123140066 |
| **Mata Kuliah** | IF25-40305 Sistem Teknologi Multimedia |
| **Repositori** | `stm-if25-40305-123140066` |

---

## Daftar Tugas

| Pertemuan | Topik | Folder | Status |
|:---:|---|---|:---:|
| 02 | Analisis Sinyal Suara & Noise Statis | [`02_audio_noise_statis/`](02_audio_noise_statis/) | Selesai |
| — | *(tugas berikutnya)* | — | — |

---

## Struktur Repositori

```
stm-if25-40305-123140066/
├── README.md                      # Halaman ini (profil + daftar tugas)
├── requirements.txt               # Dependensi Python seluruh tugas
├── .gitignore
└── 02_audio_noise_statis/         # Tugas 2
    ├── tugas_audio_noise_statis.ipynb   # Notebook lengkap (sudah dieksekusi)
    ├── tugas_audio_noise_statis.pdf     # Ekspor PDF notebook
    ├── audio_original.wav               # Rekaman asli (48 kHz)
    ├── audio_downsampled_naive_8k.wav   # Downsample naive → 8 kHz (tanpa filter)
    ├── audio_downsampled_clean_8k.wav   # Resampling terfilter → 8 kHz
    ├── audio_downsampled_naive_5k.wav # Downsample naive → 5 kHz (tanpa filter)
    ├── audio_downsampled_clean_5k.wav # Resampling terfilter → 5 kHz
    └── README.md                        # Catatan perangkat & sumber noise
```

---

## Menyiapkan Environment

Butuh **Python 3.13** dan [`uv`](https://docs.astral.sh/uv/).

```bash
# 1. Buat virtual environment di root repositori
uv venv --python 3.13

# 2. Pasang seluruh dependensi
uv pip install -r requirements.txt

# 3. (Opsional) daftarkan kernel Jupyter
.venv/Scripts/python.exe -m ipykernel install --user \
    --name audio-noise --display-name "Python (audio-noise)"
```

Aktivasi shell:

| Shell | Perintah |
|---|---|
| PowerShell | `.venv\Scripts\Activate.ps1` |
| CMD | `.venv\Scripts\activate.bat` |
| Git Bash | `source .venv/Scripts/activate` |

Menjalankan notebook:

```bash
.venv/Scripts/python.exe -m jupyter lab
```

Lalu pilih kernel **Python (audio-noise)**.

`requirements.txt` berada di root repositori karena dipakai bersama oleh seluruh tugas pada mata kuliah ini.
