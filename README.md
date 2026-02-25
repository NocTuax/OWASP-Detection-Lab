# 🚨 React2Shell (CVE-2025-55182) Detection Lab

Repositori ini adalah dokumentasi simulasi dan deteksi kerentanan **React2Shell (CVE-2025-55182)** menggunakan pendekatan *Blue Team* dan **Wazuh SIEM/EDR**. Fokus utama proyek ini adalah membangun *Detection Engineering* untuk menangkap aktivitas *Insecure Deserialization* pada aplikasi berbasis Node.js/React.

## 🎯 Objektif Lab
1. **Simulation**: Membangun aplikasi rentan untuk memahami *attack vector* (penggunaan payload Base64 dan eksekusi fungsi arbitrer).
2. **Log Analysis**: Menganalisis *behavior* aplikasi saat eksploitasi terjadi (RCE dan Reverse Shell).
3. **Detection Engineering**: Membuat *custom rule* kustom di Wazuh untuk menangkap *Indicator of Compromise* (IOC) spesifik dari eksploitasi ini secara *real-time*.

## 🛠️ Tools yang Digunakan
* **SIEM**: Wazuh Manager & Agent
* **Environment**: Ubuntu Server (Target), Kali Linux (Attacker)
* **Application**: Node.js, Express, `node-serialize`
* **Network**: Netcat (untuk simulasi Reverse Shell)

## 📂 Struktur Proyek
* `/app`: Berisi *source code* aplikasi Node.js yang rentan.
* `/payloads`: Script generator untuk membuat payload Base64 jahat.
* `/wazuh-rules`: Konfigurasi XML untuk *custom detection rules* di Wazuh.
* `/evidence`: Bukti *screenshot* dari alert Wazuh (Level 13 Critical).

---
**Author**: Muhamad Yusril Malakaini
**Role**: Aspiring SOC Analyst | Blue Team Enthusiast
**Year**: 2026
