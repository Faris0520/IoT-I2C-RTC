# Push Button

Varian dari proyek IoT I2C RTC yang mengatur jam dan menit lewat dua tombol fisik, tanpa perlu komputer. Waktu dibaca dari RTC DS1307 dan ditampilkan pada LCD 16x2 I2C.

## Rangkaian

LCD dan RTC berbagi satu bus I2C:

| ESP32 | LCD I2C | RTC DS1307 |
|-------|---------|------------|
| GPIO 21 (SDA) | SDA | SDA |
| GPIO 22 (SCL) | SCL | SCL |
| 5V | VCC | 5V |
| GND | GND | GND |

Kedua tombol dibaca dengan mode `INPUT_PULLUP`, satu sisi ke pin ESP32 dan sisi lain ke GND:

| ESP32 | Tombol |
|-------|--------|
| GPIO 18 | Menambah jam |
| GPIO 19 | Menambah menit |

## Cara kerja

Tombol pada GPIO 18 menambah satu jam setiap ditekan dan memutar kembali ke 00 setelah 23. Tombol pada GPIO 19 menambah satu menit; ketika menit mencapai 60 ia kembali ke 00 dan jam ikut bertambah satu.

Kedua tombol memakai debounce 200 milidetik agar satu tekanan tidak terbaca berkali-kali. Jika RTC belum pernah diatur, ditandai oleh `!rtc.isrunning()`, program menyetel waktu awal ke waktu kompilasi.

Setiap perubahan juga dicetak ke Serial Monitor, misalnya:

```
Jam → 20
Menit → 20:01
```

## Tampilan LCD

```
2026/09/24 Sab
19:00:00
```

Baris pertama berisi tanggal `YYYY/MM/DD` dan nama hari. Baris kedua berisi waktu `HH:MM:SS`, dengan pemisah `:` yang berkedip tiap detik.

## Menjalankan

```bash
cd "Push Button"
pio run                   # build
pio run --target upload   # upload ke board
pio device monitor -b 115200
```

Simulasi tanpa perangkat fisik tersedia lewat Wokwi menggunakan `diagram.json`.

## Library

- `marcoschwartz/LiquidCrystal_I2C@^1.1.4`
- `adafruit/RTClib`
