# 🛡️ A01: Broken Authentication - Deteksi Serangan Brute Force dengan Hydra

[![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-005571?style=flat&logo=wazuh&logoColor=white)](https://wazuh.com/)
[![OWASP Top 10](https://img.shields.io/badge/OWASP%20Top%2010-Broken%20Authentication-red)](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-T1110%20(Brute%20Force)-purple)](https://attack.mitre.org/techniques/T1110/)

## 📋 Ringkasan Proyek

Dokumentasi ini menunjukkan keberhasilan implementasi **Wazuh SIEM** untuk mendeteksi serangan **Brute Force** secara real-time pada lingkungan lab cybersecurity. Serangan ini termasuk dalam kategori **A01: Broken Authentication** pada OWASP Top 10.

## 🛠️ Arsitektur Lab

- **Attacker**: Kali Linux (IP: 192.168.15.138)
- **SIEM Manager**: Wazuh Manager (Ubuntu 22.04)
- **Target/Agent**: Ubuntu Server 22.04 dengan Apache2 & DVWA (IP: 192.168.15.139)

## ⚙️ Konfigurasi Deteksi (Wazuh Rule)

Saya membuat *custom rule* pada file `/var/ossec/etc/rules/local_rules.xml` di Wazuh Manager. Rule ini menggunakan **Regex Case-Insensitive** untuk menangkap identitas Hydra pada *User-Agent* di log akses Apache.

> **📁 Lokasi file rule**: [`../rules/local_rules.xml`](../rules/local_rules.xml)

### Custom Rule (ID: 100002)

```xml
<rule id="100002" level="10">
  <if_sid>31100,31101,31108</if_sid>
  <regex type="pcre2">(?i)hydra</regex>
  <description>🚨 [A01: Broken Authentication] Serangan Brute Force Web (Hydra) Terdeteksi!</description>
  <mitre>
    <id>T1110</id>
  </mitre>
</rule>
```
🔍 Penjelasan Rule:
Komponen	Nilai	Keterangan
ID	100002	ID unik untuk rule kustom
Level	10	Tingkat keparahan sangat tinggi (alert kritis)
if_sid	31100,31101,31108	Hanya dievaluasi untuk event dari group web server
regex	(?i)hydra	Mencari string "hydra" (case-insensitive) di User-Agent
mitre	T1110	Mapping ke teknik Brute Force di MITRE ATT&CK
🚀 Skenario Pengujian
Langkah 1: Menjalankan Serangan (Dari Attacker)
Di mesin Kali Linux (192.168.15.138), jalankan perintah Hydra:

bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.15.139 http-post-form "/DVWA/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
Screenshot eksekusi Hydra di Kali Linux:

https://./images/hydra-attack-terminal.png

Langkah 2: Log Akses Apache di Target
Setiap percobaan login dari Hydra tercatat di file /var/log/apache2/access.log pada mesin target (Ubuntu Server).

Screenshot log akses Apache yang menunjukkan aktivitas Hydra:

https://./images/apache-access-log-hydra.png

Langkah 3: Proses Deteksi oleh Wazuh
Logging: Setiap percobaan login dari Hydra tercatat di log akses Apache

Forwarding: Wazuh Agent mengirim log ke Wazuh Manager

Analysis: Wazuh Manager mencocokkan dengan rule yang ada

Alerting: Rule ID 100002 terpicu → alert level 10 dihasilkan

Langkah 4: Hasil Deteksi di Wazuh Dashboard
Alert langsung muncul di Wazuh Dashboard dengan level kritis (10).

Screenshot alert di Wazuh Dashboard:

https://./images/wazuh-dashboard-alert.png

Detail alert yang muncul:

text
Rule ID    : 100002
Level      : 10
Description: 🚨 [A01: Broken Authentication] Serangan Brute Force Web (Hydra) Terdeteksi!
Source IP  : 192.168.15.138 (Attacker)
Target IP  : 192.168.15.139 (Target)
Screenshot tambahan:

https://./images/Screenshot%25202026-03-05%2520085813.png
https://./images/Screenshot%25202026-03-05%2520090306.png

📊 Mapping MITRE ATT&CK
Taktik	Teknik	ID
Credential Access	Brute Force	T1110
✅ Kesimpulan
Proyek ini berhasil mendemonstrasikan:

Deteksi Dini: Wazuh mampu mendeteksi serangan Brute Force secara real-time

Kustomisasi Rule: Rule spesifik untuk alat Hydra berhasil diimplementasikan

Konteks Ancaman: Mapping ke MITRE ATT&CK (T1110) meningkatkan nilai analisis

OWASP Top 10: Validasi deteksi untuk kategori Broken Authentication

📁 File Pendukung
Custom Rule: ../rules/local_rules.xml

Screenshots: ./images/

🖇️ Navigasi
Kembali ke README Utama

Dokumentasi ini disusun sebagai bagian dari portofolio keamanan siber.
