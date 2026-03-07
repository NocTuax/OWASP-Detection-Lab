# 🛡️ Project #1: A07:2025 - Authentication Failures (Brute Force Hydra)

> **🔍 Catatan:** Project ini mendeteksi serangan **A07: Authentication Failures** dari OWASP Top 10 2025. Penomoran folder `01` adalah urutan project, bukan kode OWASP.

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
Nih langsung **copy-paste** dari **Penjelasan Rule** (tanpa file XML) sampai akhir:

---

### 🔍 Penjelasan Rule:

| Komponen | Nilai | Keterangan |
|----------|-------|------------|
| **ID** | `100002` | ID unik untuk rule kustom |
| **Level** | `10` | Tingkat keparahan sangat tinggi (alert kritis) |
| **if_sid** | `31100,31101,31108` | Hanya dievaluasi untuk event dari group web server |
| **regex** | `(?i)hydra` | Mencari string "hydra" (case-insensitive) di User-Agent |
| **mitre** | `T1110` | Mapping ke teknik Brute Force di MITRE ATT&CK |

## 🚀 Skenario Pengujian

### Langkah 1: Menjalankan Serangan (Dari Attacker)

Di mesin Kali Linux (`192.168.15.138`), jalankan perintah Hydra:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.15.139 http-post-form "/DVWA/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
```

**Penjelasan perintah:**
- `-l admin` → Username target (admin)
- `-P /usr/share/wordlists/rockyou.txt` → Wordlist password
- `http-post-form` → Metode serangan (HTTP POST form)
- `Login failed` → String indikator login gagal

**Screenshot eksekusi Hydra di Kali Linux:**

![Hydra Attack Terminal](./images/hydra-attack-terminal.png)

### Langkah 2: Log Akses Apache di Target

Setiap percobaan login dari Hydra tercatat di file `/var/log/apache2/access.log` pada mesin target (Ubuntu Server). Perhatikan **User-Agent** yang menunjukkan identitas Hydra.

**Screenshot log akses Apache yang menunjukkan aktivitas Hydra:**

![Apache Access Log Hydra](./images/apache-access-log-hydra.png)

### Langkah 3: Proses Deteksi oleh Wazuh

1. **Logging**: Setiap percobaan login dari Hydra tercatat di log akses Apache
2. **Forwarding**: Wazuh Agent membaca log dan mengirim ke Wazuh Manager secara real-time
3. **Analysis**: Wazuh Manager menerima log, mem-parsing, dan mencocokkannya dengan rule yang ada
4. **Alerting**: Karena ada kecocokan dengan **Rule ID 100002** (adanya string "hydra" di User-Agent), maka alert level 10 langsung dihasilkan

### Langkah 4: Hasil Deteksi di Wazuh Dashboard

Alert langsung muncul di **Wazuh Dashboard** dengan level kritis (10).

**Screenshot alert di Wazuh Dashboard:**

![Wazuh Dashboard Alert](./images/wazuh-dashboard-alert.png)

Detail alert yang muncul:

```
Rule ID    : 100002
Level      : 10
Description: 🚨 [A01: Broken Authentication] Serangan Brute Force Web (Hydra) Terdeteksi!
Source IP  : 192.168.15.138 (Attacker)
Target IP  : 192.168.15.139 (Target)
```

## 📊 Mapping MITRE ATT&CK

| Taktik | Teknik | ID |
|--------|--------|-----|
| Credential Access | Brute Force | [T1110](https://attack.mitre.org/techniques/T1110/) |

Dengan mapping ini, alert tidak hanya memberi tahu *apa* yang terjadi, tetapi juga *di mana* posisinya dalam siklus hidup serangan menurut standar industri.

## ✅ Kesimpulan

Proyek ini berhasil mendemonstrasikan:

1. **Deteksi Dini**: Wazuh mampu mendeteksi serangan Brute Force secara real-time, memungkinkan respons cepat sebelum akun berhasil dikompromikan
2. **Kustomisasi Rule**: Kemampuan membuat rule kustom sangat penting untuk mendeteksi alat serangan spesifik (Hydra) yang mungkin tidak terdeteksi oleh rule bawaan
3. **Konteks Ancaman**: Mapping ke MITRE ATT&CK (T1110) memberikan nilai tambah dengan menempatkan serangan dalam kerangka yang diakui secara global
4. **Penerapan OWASP Top 10**: Berhasil memvalidasi bahwa serangan *Broken Authentication* dapat dimonitor dan dideteksi secara efektif

## 📁 File Pendukung

- **Custom Rule**: [`../rules/local_rules.xml`](../rules/local_rules.xml) - File XML berisi rule deteksi Hydra
- **Screenshots**: [`./images/`](./images/) - Koleksi screenshot proses serangan dan deteksi

## 🖇️ Navigasi

- [**Kembali ke README Utama**](../README.md) - Lihat semua skenario OWASP Top 10

---
*Dokumentasi ini disusun sebagai bagian dari portofolio keamanan siber. Fokus: SIEM Engineering, Threat Detection, dan OWASP Top 10.*
