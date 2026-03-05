# 🛡️ OWASP Detection Lab - Brute Force (Hydra)

Dokumentasi ini menunjukkan keberhasilan implementasi **Wazuh SIEM** untuk mendeteksi serangan **Brute Force** secara real-time pada lingkungan lab cybersecurity.

## 📋 Ringkasan Proyek
Proyek ini berfokus pada pemantauan log akses Apache untuk mengidentifikasi aktivitas mencurigakan dari alat penetrasi populer, yaitu **Hydra**, sesuai dengan kategori serangan **Broken Authentication** pada OWASP Top 10.

## 🛠️ Arsitektur Lab
* **Attacker**: Kali Linux (IP: 192.168.15.138)
* **SIEM Manager**: Wazuh Manager (Ubuntu 22.04)
* **Target/Agent**: Ubuntu Server 22.04 dengan Apache2 & DVWA (IP: 192.168.15.139)

## 🛡️ Konfigurasi Deteksi
Saya mengimplementasikan *Custom Rules* pada Wazuh Manager (`local_rules.xml`) menggunakan **Regex Case-Insensitive** untuk menangkap User-Agent unik yang dikirimkan oleh Hydra.

### Custom Rule (ID: 100002):
```xml
<rule id="100002" level="10">
  <if_sid>31100,31101,31108</if_sid>
  <regex type="pcre2">(?i)hydra</regex>
  <description>Waspada! Serangan Brute Force Web (Hydra) Terdeteksi!</description>
  <mitre>
    <id>T1110</id>
  </mitre>
</rule>

