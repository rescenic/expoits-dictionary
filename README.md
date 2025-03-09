# Kamus Besar Bypass WAF

## 1. Pendahuluan
Web Application Firewall (WAF) adalah sistem keamanan yang melindungi aplikasi web dari berbagai ancaman, termasuk SQL Injection, XSS, dan serangan lainnya. Namun, ada berbagai teknik untuk melewati atau memanipulasi WAF agar serangan tetap berhasil.

## 2. Pengertian Bypass WAF
Bypass WAF adalah teknik yang digunakan untuk menghindari deteksi oleh firewall aplikasi web dengan cara memodifikasi payload atau mengeksploitasi kelemahan dalam sistem deteksi WAF.

## 3. Teknik-Teknik Bypass WAF

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

## 4. Dampak dari Bypass WAF
- Data sensitif dapat dicuri.
- Website dapat mengalami defacement atau DDoS.
- Serangan dapat menyebabkan kebocoran informasi pengguna.

## 5. Pencegahan dan Mitigasi
- Gunakan WAF yang lebih canggih dengan machine learning.
- Terapkan input validation yang ketat.
- Monitoring traffic untuk mendeteksi pola aneh.

## 6. Kesimpulan
Bypass WAF adalah teknik yang terus berkembang. WAF yang lemah dapat dengan mudah dilewati oleh teknik yang lebih canggih. Oleh karena itu, penting untuk selalu mengupdate sistem keamanan dan memahami berbagai teknik yang digunakan oleh penyerang.

## 7. Referensi dan Sumber Belajar
- [OWASP](https://owasp.org/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
