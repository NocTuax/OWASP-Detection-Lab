# 🛡️ A01: Broken Authentication - Deteksi Serangan Brute Force dengan Hydra

[![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-005571?style=flat&logo=wazuh&logoColor=white)](https://wazuh.com/)
[![OWASP Top 10](https://img.shields.io/badge/OWASP%20Top%2010-Broken%20Authentication-red)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-T1110%20(Brute%20Force)-purple)](https://attack.mitre.org/techniques/T1110/)

## 📋 Ringkasan Proyek

Ini adalah skenario pertama dari seri **OWASP Detection Lab**. Proyek ini mendokumentasikan implementasi **Wazuh SIEM** untuk mendeteksi serangan **Brute Force** yang termasuk dalam kategori **A01: Broken Authentication** pada OWASP Top 10.

Serangan dilakukan menggunakan alat **Hydra** terhadap aplikasi web DVWA, dan berhasil dideteksi secara *real-time* melalui *custom rule* yang dibuat di Wazuh Manager.

## 🛠️ Arsitektur Lab

| Komponen | Sistem Operasi | IP Address | Peran |
|----------|----------------|------------|-------|
| **Attacker** | Kali Linux | `192.168.15.138` | Menjalankan serangan Brute Force dengan Hydra |
| **Target (Agent)** | Ubuntu Server 22.04 | `192.168.15.139` | Menjalankan Apache2 & DVWA, terpasang Wazuh Agent |
| **SIEM Manager** | Ubuntu 22.04 | (Internal) | Wazuh Manager (pusat analisis log) |

**Detail Target:**
- Aplikasi: **DVWA (Damn Vulnerable Web Application)**
- Web Server: **Apache2**
- Log yang dipantau: `/var/log/apache2/access.log`

## ⚙️ Konfigurasi Deteksi (Wazuh Rule)

Saya membuat *custom rule* pada file `/var/ossec/etc/rules/local_rules.xml` di Wazuh Manager. Rule ini menggunakan **Regex Case-Insensitive** untuk menangkap identitas Hydra pada *User-Agent* di log akses Apache.

### Custom Rule (ID: 100002)

```xml
<rule id="100002" level="10">
  <if_sid>31100,31101,31108</if_sid>
  <regex type="pcre2">(?i)hydra</regex>
  <description>🚨 [A01: Broken Authentication] Serangan Brute Force Web (Hydra) Terdeteksi!</description>
  <mitre>
    <id>T1110</id> <!-- MITRE ATT&CK: Brute Force -->
  </mitre>
</rule>
📁 Lokasi file rule: /rules/local_rules.xml

🔍 Penjelasan Rule
Komponen	Nilai	Keterangan
ID	100002	ID unik untuk rule kustom
Level	10	Tingkat keparahan sangat tinggi (alert kritis)
if_sid	31100,31101,31108	Hanya dievaluasi untuk event dari group web server
regex	(?i)hydra	Mencari string "hydra" (case-insensitive) di User-Agent
mitre	T1110	Mapping ke teknik Brute Force di MITRE ATT&CK
🚀 Skenario Pengujian
Langkah 1: Menjalankan Serangan (Dari Attacker)
Di mesin Kali Linux (192.168.15.138), jalankan perintah Hydra berikut:

bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.15.139 http-post-form "/DVWA/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
Penjelasan perintah:

-l admin → Username yang menjadi target (admin)

-P rockyou.txt → Wordlist password

http-post-form → Metode serangan (HTTP POST form)

Login failed → String indikator login gagal

Langkah 2: Proses Deteksi oleh Wazuh
Logging: Setiap percobaan login dari Hydra tercatat di log akses Apache

Forwarding: Wazuh Agent membaca log dan mengirim ke Manager

Analysis: Wazuh Manager mencocokkan dengan rule yang ada

Alerting: Rule ID 100002 terpicu → alert level 10 dihasilkan

Langkah 3: Hasil Deteksi
Setelah serangan dijalankan, alert langsung muncul di Wazuh Dashboard:

text
Rule ID    : 100002
Level      : 10
Description: 🚨 [A01: Broken Authentication] Serangan Brute Force Web (Hydra) Terdeteksi!
Source IP  : 192.168.15.138 (Attacker)
Target IP  : 192.168.15.139 (Target)
📸 Screenshot:
(Letakkan screenshot alert di folder /images dan tampilkan di sini)

Contoh: ![Alert di Wazuh Dashboard](../images/alert-hydra.png)

📊 Mapping MITRE ATT&CK
Taktik	Teknik	ID
Credential Access	Brute Force	T1110
Dengan mapping ini, alert tidak hanya memberi tahu apa yang terjadi, tetapi juga di mana posisinya dalam siklus hidup serangan menurut standar industri.

📁 File Pendukung
Custom Rule: ../rules/local_rules.xml

Screenshot: (akan ditambahkan di folder ../images/)

✅ Kesimpulan
Proyek ini berhasil mendemonstrasikan:

Deteksi Dini: Wazuh mampu mendeteksi serangan Brute Force secara real-time

Kustomisasi Rule: Rule spesifik untuk alat Hydra berhasil diimplementasikan

Konteks Ancaman: Mapping ke MITRE ATT&CK (T1110) meningkatkan nilai analisis

OWASP Top 10: Validasi deteksi untuk kategori Broken Authentication

🖇️ Navigasi
Kembali ke README Utama (lihat skenario OWASP lainnya)

Lihat Folder Rules (koleksi custom rules)

Dokumentasi ini disusun sebagai bagian dari portofolio keamanan siber. Fokus: SIEM Engineering, Threat Detection, dan OWASP Top 10.
