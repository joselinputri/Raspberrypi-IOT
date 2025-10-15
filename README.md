# Raspberrypi-IOT
# 📑 Laporan Resmi
**Proyek:** Monitoring Jarak dengan Sensor Ultrasonik & ESP32, Upload Data ke ThingSpeak  

---

## 👥 Anggota Kelompok
| Nama                     | NRP        |
|---------------------------|------------|
| Shinta Alya Ramadani      | 5027241016 |
| Nisrina Bilqis            | 5027241054 |
| Putri Joselina Silitonga  | 5027241116 |

---

## 📘 Deskripsi Proyek
Pada modul ini, dilakukan implementasi **sensor ultrasonik HC-SR04** yang terhubung ke **Raspberry Pi** untuk mengukur jarak suatu objek.  
Proyek ini bertujuan untuk memahami cara kerja sensor ultrasonik dalam mengukur jarak menggunakan prinsip pantulan gelombang suara (*echo*).

---

## 🧰 Alat dan Bahan
- Raspberry Pi (Model 3/4/5)  
- Sensor Ultrasonik **HC-SR04**   
- Kabel jumper (female-to-female)  
- Aplikasi terminal 
---

## ⚙️ Skema Koneksi

| Pin HC-SR04 | Pin Raspberry Pi | Keterangan |
|--------------|------------------|-------------|
| VCC | Pin 2 (5V) | Power 5V |
| GND | Pin 6 (GND) | Ground |
| TRIG | GPIO 23 (Pin 16) | Pemicu |
| ECHO | GPIO 24 (Pin 18) | Penerima (via pembagi tegangan) |


## 💻 Kode Program – `sensor_jarak.py`

Masuk ke terminal Raspberry Pi dan ketik perintah berikut:
```bash
nano sensor_jarak.py
```
Salin kode berikut pada file ``` sensor_jarak.py ```

```python
import RPi.GPIO as GPIO
import time

# Gunakan penomoran BCM
GPIO.setmode(GPIO.BCM)

# Atur GPIO
TRIG = 23
ECHO = 24

GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)

def hitung_jarak():
    # Set TRIG rendah untuk memastikan tidak ada pulsa
    GPIO.output(TRIG, False)
    time.sleep(0.5)

    # Kirim pulsa selama 10 mikrodetik
    GPIO.output(TRIG, True)
    time.sleep(0.00001)
    GPIO.output(TRIG, False)

    # Waktu sinyal mulai
    while GPIO.input(ECHO) == 0:
        waktu_mulai = time.time()

    # Waktu sinyal diterima kembali
    while GPIO.input(ECHO) == 1:
        waktu_akhir = time.time()

    durasi = waktu_akhir - waktu_mulai

    # Kecepatan suara = 34300 cm/s
    jarak = durasi * 17150
    jarak = round(jarak, 2)

    return jarak

try:
    while True:
        jarak = hitung_jarak()
        print(f"Jarak: {jarak} cm")
        time.sleep(1)

except KeyboardInterrupt:
    print("Menghentikan program")
    GPIO.cleanup()
```

# 🧪 Hasil Uji Coba

Program dijalankan dengan perintah berikut:

```bash
python3 sensor_jarak.py
```

# 📟 Output Terminal
```bash
Jarak: 158.48 cm
Jarak: 177.6 cm
Jarak: 0.1 cm
Jarak: 231.12 cm
Jarak: 0.35 cm
Jarak: 91.64 cm
```

# 📊 Analisis

1. Nilai jarak berubah-ubah tergantung jarak objek di depan sensor.
2. Kadang muncul pembacaan sangat kecil (misal: 0.1 cm) akibat pantulan tidak stabil atau interferensi sinyal.
3. Sensor bekerja dengan baik saat objek memiliki permukaan keras dan rata.

# 🔌 Dokumentasi Hardware

| Tampilan | Gambar |
|-----------|--------|
| Rangkaian HC-SR04 terhubung ke Raspberry Pi | ![Setup 1](./WhatsApp%20Image%202025-10-15%20at%2011.27.42.jpeg) |
| Sudut lain rangkaian | ![Setup 2](./WhatsApp%20Image%202025-10-15%20at%2011.27.43.jpeg) |
| Hasil pembacaan di terminal | ![Terminal Output](./WhatsApp%20Image%202025-10-15%20at%2011.28.42.jpeg) |

---

