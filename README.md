# IoT I2C RTC

Jam digital berbasis ESP32 yang membaca waktu dari modul RTC DS1307 dan menampilkannya pada LCD 16x2 lewat antarmuka I2C. Karena waktu disimpan di RTC, jam tetap berjalan dengan benar setelah board di-reset atau kehilangan daya sesaat.

Repository ini berisi dua program terpisah yang berbeda hanya pada cara mengatur waktu:

| Folder | Cara mengatur waktu |
|--------|---------------------|
| [`Push Button`](./Push%20Button) | Dua tombol fisik untuk menambah jam dan menit |
| [`Serial Monitor`](./Serial%20Monitor) | Perintah teks `SETTING hh:mm:ss` lewat Serial Monitor |

Keduanya memakai rangkaian dan library yang sama. Pilih varian sesuai kebutuhan: tombol fisik jika perangkat berdiri sendiri, atau Serial Monitor jika waktu diatur dari komputer.

## Yang dibutuhkan

Perangkat keras:

- Board ESP32 (dikonfigurasi sebagai `featheresp32` di PlatformIO)
- Modul RTC DS1307
- LCD 16x2 dengan modul I2C, alamat `0x27`
- Dua push button (hanya untuk varian Push Button)
- Kabel jumper

Perangkat lunak dan library:

- [PlatformIO](https://platformio.org/), sebagai extension VS Code atau lewat CLI
- `marcoschwartz/LiquidCrystal_I2C@^1.1.4`
- `adafruit/RTClib`

Kedua library terpasang otomatis saat build karena sudah tercantum di `platformio.ini` masing-masing folder. Simulasi tanpa perangkat fisik bisa dijalankan lewat [Wokwi](https://wokwi.com/); tiap folder sudah menyediakan `diagram.json` dan `wokwi.toml`.

## Rangkaian

LCD dan RTC berbagi satu bus I2C yang sama:

| ESP32 | LCD I2C | RTC DS1307 |
|-------|---------|------------|
| GPIO 21 (SDA) | SDA | SDA |
| GPIO 22 (SCL) | SCL | SCL |
| 5V | VCC | 5V |
| GND | GND | GND |

Varian Push Button menambah dua tombol yang dibaca dengan mode `INPUT_PULLUP`, satu sisi ke pin ESP32 dan sisi lain ke GND:

| ESP32 | Tombol |
|-------|--------|
| GPIO 18 | Menambah jam |
| GPIO 19 | Menambah menit |

## Tampilan LCD

Baris pertama menampilkan tanggal dan nama hari, baris kedua menampilkan waktu:

```
2026/09/24 Sab
19:00:00
```

Tanggal memakai format `YYYY/MM/DD`. Nama hari dipendekkan menjadi Min, Sen, Sel, Rab, Kam, Jum, dan Sab. Pada varian Push Button, pemisah `:` pada baris waktu berkedip setiap detik sebagai tanda jam masih berjalan.

## Menjalankan

Masuk ke folder varian yang dipilih, lalu build dan upload lewat PlatformIO:

```bash
cd "Push Button"          # atau "Serial Monitor"
pio run                   # build
pio run --target upload   # upload ke board
pio device monitor -b 115200
```

Baca README di dalam tiap folder untuk detail cara kerja dan cara mengatur waktu pada varian tersebut.

## Struktur repository

```
IoT-I2C-RTC/
├── Push Button/
│   ├── src/main.cpp        Program pengatur waktu lewat tombol
│   ├── diagram.json        Skema rangkaian untuk Wokwi
│   ├── platformio.ini      Konfigurasi board dan library
│   └── wokwi.toml
├── Serial Monitor/
│   ├── src/main.cpp        Program pengatur waktu lewat Serial
│   ├── diagram.json        Skema rangkaian untuk Wokwi
│   ├── platformio.ini      Konfigurasi board dan library
│   └── wokwi.toml
└── README.md
```

## Penyusun

Faris Daffa, [@Faris0520](https://github.com/Faris0520).
