# Serial Monitor

Varian dari proyek IoT I2C RTC yang mengatur waktu lewat perintah teks di Serial Monitor. Waktu dibaca dari RTC DS1307 dan ditampilkan pada LCD 16x2 I2C.

## Rangkaian

LCD dan RTC berbagi satu bus I2C:

| ESP32 | LCD I2C | RTC DS1307 |
|-------|---------|------------|
| GPIO 21 (SDA) | SDA | SDA |
| GPIO 22 (SCL) | SCL | SCL |
| 5V | VCC | 5V |
| GND | GND | GND |

## Perintah

Buka Serial Monitor pada baud rate 115200, lalu kirim perintah dengan format:

```
SETTING hh:mm:ss
```

Contoh mengatur waktu ke pukul 12:30:45:

```
SETTING 12:30:45
```

Jika berhasil, program membalas:

```
[SUKSES] Waktu RTC berhasil diatur menjadi: 12:30:45
```

Jika formatnya salah atau nilainya di luar rentang (jam 0 sampai 23, menit dan detik 0 sampai 59), program membalas:

```
[GAGAL] Format salah. Gunakan: SETTING hh:mm:ss
Contoh: SETTING 12:30:45
```

Pada `setup()`, program lebih dulu menyetel RTC ke waktu kompilasi. Sesudah itu waktu bisa diubah kapan saja lewat perintah `SETTING`.

## Tampilan LCD

```
2026/09/24
12:30:45
```

Baris pertama berisi tanggal `YYYY/MM/DD`, baris kedua berisi waktu `HH:MM:SS` yang diperbarui tiap detik.

## Menjalankan

```bash
cd "Serial Monitor"
pio run                   # build
pio run --target upload   # upload ke board
pio device monitor -b 115200
```

Simulasi tanpa perangkat fisik tersedia lewat Wokwi menggunakan `diagram.json`.

## Library

- `marcoschwartz/LiquidCrystal_I2C@^1.1.4`
- `adafruit/RTClib`
