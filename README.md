# 🛡️ OWASP Detection Lab Series: Real-Time Threat Detection with Wazuh SIEM

[![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-005571?style=flat&logo=wazuh&logoColor=white)](https://wazuh.com/)
[![OWASP Top 10](https://img.shields.io/badge/Project-OWASP%20Top%2010%20Detection-blue)](https://owasp.org/Top10/)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-Mapping-purple)](https://attack.mitre.org/)

## 📌 Tentang Proyek Ini

Repositori ini merupakan **koleksi portofolio lab keamanan siber** yang berfokus pada **implementasi Wazuh SIEM** untuk mendeteksi berbagai serangan yang termasuk dalam daftar **OWASP Top 10**.

Setiap serangan yang disimulasikan akan dikonfigurasi aturan deteksinya di Wazuh, kemudian di-mapping ke kerangka **MITRE ATT&CK** untuk memberikan konteks ancaman yang lebih mendalam. Tujuannya adalah untuk mendemonstrasikan kemampuan dalam *Security Monitoring*, *SIEM Engineering*, dan *Threat Detection*.

## 📚 Daftar Skenario Deteksi (OWASP Top 10)

Proyek ini akan terus dikembangkan untuk mencakup lebih banyak kategori OWASP Top 10. Berikut adalah skenario yang sudah atau akan diimplementasikan:

| Kode | Kategori OWASP | Vektor Serangan | Status | Dokumentasi |
|:----:|----------------|-----------------|--------|-------------|
| **A01** | Broken Authentication | Brute Force Attack dengan Hydra | ✅ Selesai | [Lihat Detil](./01-Broken-Authentication.md) |
| A02 | Cryptographic Failures | *(Coming Soon)* | ⏳ Rencana | - |
| A03 | Injection | *(Coming Soon)* | ⏳ Rencana | - |
| A04 | Insecure Design | *(Coming Soon)* | ⏳ Rencana | - |
| A05 | Security Misconfiguration | *(Coming Soon)* | ⏳ Rencana | - |
| *...dan seterusnya...* | | | | |

> **Catatan**: Setiap dokumentasi mencakup arsitektur lab, konfigurasi rule Wazuh, hasil deteksi, serta mapping ke MITRE ATT&CK.

## 🧪 Arsitektur Lab Umum

Lingkungan lab yang digunakan relatif konsisten untuk setiap skenario, dengan komponen sebagai berikut:

*   **Attacker Machine**: Kali Linux (IP Dinamis)
*   **SIEM Manager**: Ubuntu 22.04 - Wazuh Manager (pusat analisis log)
*   **Target Machine**: Ubuntu Server 22.04 - Menjalankan aplikasi rentan (DVWA, dll) + Wazuh Agent

Detail spesifik untuk setiap skenario (seperti IP Address dan tools yang digunakan) akan dijelaskan lebih lanjut di masing-masing dokumentasi.

## 🎯 Tujuan Proyek

1.  **Membangun Portofolio**: Menunjukkan kemampuan praktis dalam mengimplementasikan SIEM untuk deteksi ancaman.
2.  **Mendemonstrasikan Pemahaman OWASP Top 10**: Tidak hanya memahami teorinya, tetapi juga bisa mendeteksi serangan yang sesuai dengan setiap kategori.
3.  **Penerapan MITRE ATT&CK**: Menghubungkan alert dengan taktik dan teknik dalam framework MITRE ATT&CK untuk analisis yang lebih standar.
4.  **Dokumentasi Terstruktur**: Menyediakan panduan yang jelas dan rapi untuk setiap skenario deteksi.

## 🖇️ Navigasi Cepat

*   Mulai dengan skenario pertama: [**A01: Broken Authentication - Deteksi Brute Force Hydra**](./01-Broken-Authentication.md)

## 🙋‍♂️ Tentang Pembuat

Proyek ini dikembangkan sebagai bagian dari portofolio keamanan siber. Fokus utama: *Security Operations Center (SOC)*, *SIEM Engineering*, *Threat Hunting*, dan *Detection Rule Writing*.

---
*Untuk informasi lebih lanjut, diskusi, atau kolaborasi, silakan hubungi.*
