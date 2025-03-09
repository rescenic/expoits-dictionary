# 📖 Kamus Besar Bypass WAF

## 1. Pendahuluan
### Apa itu WAF?
Web Application Firewall (WAF) adalah sistem keamanan yang melindungi aplikasi web dari berbagai ancaman seperti SQL Injection, Cross-Site Scripting (XSS), dan serangan lainnya. WAF bekerja dengan cara memantau, menyaring, dan memblokir lalu lintas HTTP yang mencurigakan.

Namun, ada berbagai teknik untuk **melewati atau memanipulasi WAF**, memungkinkan serangan tetap berhasil tanpa terdeteksi.

### Mengapa Bypass WAF Diperlukan?
- **Pengujian keamanan**: Pentester dan tim keamanan perlu menguji kelemahan WAF.
- **Analisis kelemahan sistem**: Mengetahui celah keamanan WAF membantu dalam meningkatkan perlindungan.
- **Eksploitasi serangan**: Penyerang menggunakan teknik ini untuk menyusup ke dalam sistem yang seharusnya aman.

---

## 2. Jenis dan Kategori WAF

### a. Berdasarkan Implementasi
1. **Network-based WAF**: Dipasang sebagai hardware pada jaringan.
2. **Host-based WAF**: Terpasang langsung di dalam aplikasi.
3. **Cloud-based WAF**: Dikelola oleh penyedia layanan seperti Cloudflare, AWS Shield, dan Akamai.

### b. Berdasarkan Metode Analisis
1. **Signature-based WAF**: Memeriksa pola serangan berdasarkan database tanda tangan (signatures).
2. **Behavioral-based WAF**: Menganalisis perilaku lalu lintas untuk mendeteksi anomali.
3. **AI-powered WAF**: Menggunakan machine learning untuk mengidentifikasi ancaman baru.

---

## 3. Teknik-Teknik Bypass WAF 🔥

### a. Obfuscation (Pengaburan Payload)
Menggunakan encoding seperti URL encoding, Base64, atau rotasi karakter untuk menyamarkan payload.

**Contoh:**
```sql
SELECT * FROM users WHERE username='admin' -- 
```
Bisa diubah menjadi:
```sql
SELECT%20*%20FROM%20users%20WHERE%20username%3D'admin'%20--
```

### b. Case Manipulation
Mengubah huruf besar dan kecil dalam query SQL untuk mengelabui deteksi berbasis pola.

**Contoh:**
```sql
sElEcT * fRoM users WHEre username='admin' --
```

### c. WAF Evasion dengan JSON
Beberapa WAF tidak memfilter query SQL yang menggunakan JSON.

**Contoh:**
```sql
SELECT * FROM users WHERE username={"username":"admin"}
```

### d. Polyglot Payloads
Menggunakan payload yang dapat bekerja dalam berbagai encoding.

**Contoh:**
```sql
/*!50000SELECT*/ * FROM users
```

### e. Time-Based Bypass (SQL Injection)
Jika WAF memblokir pesan error, kita bisa menggunakan teknik time delay untuk mendeteksi keberadaan SQL Injection.

**Contoh:**
```sql
SELECT IF(1=1, SLEEP(5), 0)
```

### f. Bypass XSS Protection
Menggunakan teknik seperti event handler atau penyusupan inline script untuk menghindari filter WAF.

**Contoh:**
```html
<svg/onload=alert('XSS')>
```

### g. HTTP Parameter Pollution
Mengirimkan parameter duplikat untuk mengelabui parser WAF.

**Contoh:**
```
GET /index.php?id=1&id=2
```

---

## 4. Analisis Cara Kerja WAF dan Weakness Exploitation

### a. Bagaimana WAF Mendeteksi Serangan?
- **Pattern Matching**: Membandingkan input dengan daftar pola yang diketahui.
- **Heuristic Analysis**: Menganalisis struktur permintaan HTTP untuk mendeteksi anomali.
- **Behavioral Analysis**: Memantau perilaku pengguna untuk mendeteksi pola mencurigakan.

### b. Teknik Khusus untuk Cloud WAF
- **Cloudflare Bypass**: Menggunakan domain asli untuk melewati proteksi.
- **AWS WAF Bypass**: Mengeksploitasi kebijakan IAM yang salah konfigurasi.
- **Akamai Bypass**: Menggunakan header HTTP yang dimodifikasi.

---

## 5. Dampak dari Bypass WAF ⚠️

- **Pencurian Data**: Informasi pengguna dapat dicuri dengan serangan SQL Injection.
- **Defacement Website**: Hacker dapat mengubah tampilan situs dengan Web Shell.
- **Akses Tidak Sah**: Penyerang bisa mendapatkan akses administrator.
- **Serangan DDoS**: WAF yang lemah bisa gagal melindungi dari serangan volumetrik.

---

## 6. Pencegahan dan Mitigasi 🛡️

- **Gunakan WAF dengan AI/ML** untuk mendeteksi pola serangan baru.
- **Terapkan Input Validation** agar hanya data yang valid diterima.
- **Monitor Log Secara Aktif** untuk mendeteksi percobaan serangan.
- **Gunakan Rate Limiting** untuk membatasi jumlah permintaan mencurigakan.

---

## 7. Kesimpulan
Bypass WAF adalah teknik yang berkembang seiring dengan meningkatnya sistem keamanan. Pentester dan admin keamanan harus selalu memperbarui metode perlindungan agar tetap aman dari serangan.

---

## 8. Referensi dan Sumber Belajar 📚
- [OWASP](https://owasp.org/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [Cloudflare WAF](https://developers.cloudflare.com/waf/)

