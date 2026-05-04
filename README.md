# PrakSismik_Aditia_H1H024014_Pert4

## 1. Proses Konversi Sinyal Analog ke Digital (ADC)

Arduino menggunakan ADC (Analog-to-Digital Converter) 10-bit yang tertanam di dalam mikrokontrolernya. Prosesnya berlangsung dalam beberapa tahap:
1. Sampling Arduino mengambil "foto" tegangan dari pin analog pada satu titik waktu tertentu. Tegangan masukan yang diterima berkisar antara 0V hingga 5V (tegangan referensi).
2. Quantization Tegangan tersebut kemudian dibagi ke dalam 1024 tingkatan (2¹⁰ = 1024). Misalnya, tegangan 0V akan menghasilkan nilai 0, dan tegangan 5V menghasilkan nilai 1023. Tegangan 2,5V akan menghasilkan sekitar nilai 511.
3. Encoding Nilai hasil quantization disimpan sebagai bilangan integer 10-bit di dalam register ADC, lalu dapat dibaca oleh program menggunakan perintah `analogRead()`.
Jadi secara singkat: tegangan analog masuk → disampling → diubah jadi angka 0–1023 → siap diproses oleh program.

---

## 2. Faktor yang Mempengaruhi Keakuratan Pembacaan ADC
1. Tegangan referensi tidak stabil ADC menggunakan tegangan referensi (biasanya 5V) sebagai patokan. Jika sumber tegangan Arduino tidak stabil atau ada noise, hasil pembacaan akan ikut berfluktuasi.
2. Resolusi ADC terbatas Dengan resolusi 10-bit, satu "langkah" ADC mewakili sekitar 4,9 mV (5V ÷ 1024). Perubahan tegangan di bawah nilai ini tidak akan terdeteksi.
3. Noise elektromagnetik Kabel dan komponen di sekitar pin analog dapat menangkap interferensi dari lingkungan, menyebabkan nilai pembacaan berubah-ubah meskipun potensiometer tidak diputar.
4. Impedansi sumber yang tinggi Jika sumber sinyal analog memiliki resistansi tinggi, kapasitor sampling ADC tidak dapat terisi dengan cepat, sehingga hasil pembacaan menjadi tidak akurat.
5. Suhu Perubahan suhu dapat memengaruhi karakteristik komponen seperti resistor pada potensiometer, sehingga nilai yang terbaca bisa bergeser secara perlahan.

---

## 3. Kendala Integrasi ADC dan PWM dalam Satu Sistem
**Interferensi sinyal PWM ke ADC** Sinyal PWM menghasilkan switching tegangan yang cepat dan berulang. Jika jalur kabel terlalu berdekatan, noise dari sinyal PWM bisa masuk ke pin ADC dan membuat pembacaan terganggu.
**Waktu sampling yang tidak tepat**  Jika `analogRead()` dilakukan tepat saat sinyal PWM sedang switching, nilai yang terbaca bisa tidak valid. Penambahan `delay()` yang cukup membantu memberi waktu sinyal stabil sebelum dibaca.

**Berbagi ground yang tidak bersih** — Arus yang mengalir ke LED melalui jalur PWM dapat menciptakan perbedaan potensial kecil pada jalur ground, yang kemudian ikut mempengaruhi tegangan referensi ADC.

**Beban berlebih pada pin** — Jika LED menarik arus terlalu besar tanpa resistor pembatas, tegangan suplai bisa turun sedikit, dan hal ini memengaruhi tegangan referensi ADC serta akurasi pembacaan.

**Keterbatasan pin PWM** — Tidak semua pin Arduino mendukung PWM. Jika salah memilih pin, `analogWrite()` tidak akan menghasilkan sinyal PWM dan LED tidak akan bisa diatur kecerahannya.
