# 🛡️ OWASP Detection Lab: Real-Time Threat Detection with Wazuh SIEM

[![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-005571?style=flat&logo=wazuh&logoColor=white)](https://wazuh.com/)
[![OWASP Top 10](https://img.shields.io/badge/OWASP%20Top%2010%202025-Detection%20Lab-blue)](https://owasp.org/Top10/)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-Mapping-purple)](https://attack.mitre.org/)

## 📌 Tentang Proyek Ini

Repositori ini merupakan **koleksi portofolio lab keamanan siber** yang mendokumentasikan implementasi **Wazuh SIEM** untuk mendeteksi berbagai serangan yang termasuk dalam daftar **OWASP Top 10 2025**.

Setiap skenario serangan dikonfigurasi dengan *custom rules* di Wazuh, diuji dalam lingkungan lab terkendali, dan di-mapping ke kerangka **MITRE ATT&CK** untuk memberikan konteks ancaman yang lebih mendalam.

> **📌 Catatan:** Penomoran folder (01, 02, 03) adalah **urutan project**, bukan kode OWASP asli. Kode OWASP asli (A01-A10) akan dicantumkan di tabel berikut.

## 📂 Struktur Repositori

```
OWASP-Detection-Lab/
├── 01-Broken-Authentication/   # Project #1: A07 - Authentication Failures
│   └── README.md                # Deteksi Brute Force dengan Hydra
├── 02-Injection/                # Project #2: A05 - Injection
│   └── README.md                # Deteksi SQL Injection dengan SQLMap
├── images/                      # Screenshot dan gambar pendukung
├── rules/                       # Kumpulan custom rules Wazuh (XML)
│   ├── local_rules.xml           # Rule untuk A07: Brute Force Hydra
│   └── sqli_rules.xml            # Rule untuk A05: SQL Injection SQLMap
└── README.md                    # Dokumentasi utama (file ini)
```

## 📋 Daftar Skenario Deteksi (OWASP Top 10 2025)

| Project | Kode OWASP | Kategori | Vektor Serangan | Status | MITRE ATT&CK | Dokumentasi |
|:-------:|:----------:|----------|-----------------|--------|--------------|-------------|
| **#1** | **A07:2025** | Authentication Failures | Brute Force dengan Hydra | ✅ Selesai | [T1110](https://attack.mitre.org/techniques/T1110/) | [01-Broken-Authentication/](./01-Broken-Authentication/) |
| **#2** | **A05:2025** | Injection | SQL Injection dengan SQLMap | ✅ Selesai | [T1190](https://attack.mitre.org/techniques/T1190/) | [02-Injection/](./02-Injection/) |
| **#3** | A02:2025 | Security Misconfiguration | *(Coming Soon)* | ⏳ Rencana | - | - |
| **#4** | A01:2025 | Broken Access Control | *(Coming Soon)* | ⏳ Rencana | - | - |
| **#5** | A03:2025 | Software Supply Chain Failures | *(Coming Soon)* | ⏳ Rencana | - | - |
| **#6** | A04:2025 | Cryptographic Failures | *(Coming Soon)* | ⏳ Rencana | - | - |

## 🧪 Arsitektur Lab Umum

Lingkungan lab yang digunakan relatif konsisten untuk setiap skenario:

| Peran | Sistem Operasi | Tools/Aplikasi |
|-------|----------------|----------------|
| **Attacker** | Kali Linux | Hydra, SQLMap, dll (sesuai skenario) |
| **Target (Agent)** | Ubuntu Server 22.04 | Apache2, DVWA, Wazuh Agent |
| **SIEM Manager** | Ubuntu 22.04 | Wazuh Manager |

Detail spesifik seperti IP Address dan konfigurasi akan dijelaskan di setiap dokumentasi skenario.

## 🛠️ Koleksi Custom Rules

Semua *custom rules* yang dibuat untuk proyek ini dapat ditemukan di folder [`/rules`](./rules/). Saat ini tersedia:

| File Rule | Project | Kode OWASP | Deteksi |
|-----------|---------|------------|---------|
| [`rules/local_rules.xml`](./rules/local_rules.xml) | #1 | A07:2025 | Brute Force dengan Hydra |
| [`rules/sqli_rules.xml`](./rules/sqli_rules.xml) | #2 | A05:2025 | SQL Injection dengan SQLMap |

## 🎯 Tujuan Proyek

1. **Membangun Portofolio** - Menunjukkan kemampuan praktis dalam SIEM Engineering dan Threat Detection
2. **Validasi OWASP Top 10 2025** - Mendemonstrasikan deteksi untuk setiap kategori risiko terbaru
3. **Penerapan MITRE ATT&CK** - Memberikan konteks ancaman standar industri
4. **Dokumentasi Terstruktur** - Menyediakan panduan yang jelas dan *replicable*

## 🚀 Mulai Jelajahi

- **[Project #1: A07 - Authentication Failures (Brute Force Hydra)](./01-Broken-Authentication/)** → Deteksi serangan Brute Force
- **[Project #2: A05 - Injection (SQL Injection SQLMap)](./02-Injection/)** → Deteksi serangan SQL Injection

## 🙋‍♂️ Tentang Pembuat

Proyek ini dikembangkan sebagai bagian dari portofolio keamanan siber. Fokus utama: **SOC Operations**, **SIEM Engineering**, **Threat Hunting**, dan **Detection Rule Writing**.

---
*Dokumentasi ini disusun sebagai bagian dari portofolio keamanan siber. Last updated: March 2026*

![Profile Views](https://komarev.com/ghpvc/?username=NocTuax&color=blue)
```
