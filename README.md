# TKA MODUL 3
# Panduan Pengerjaan Praktikan 2 & 3

Semua file untuk Praktikan 2 (Backend) dan Praktikan 3 (Frontend) sudah disiapkan (termasuk Ansible roles, variables, Jinja2 template, dan Dockerfiles).

## 1. Menjalankan Ansible Playbook
Karena instruksinya mengatakan Anda menggunakan WSL, buka terminal WSL Anda, arahkan ke folder `ansible` ini, dan jalankan perintah:

```bash
cd ansible
ansible-playbook -i inventory.yml site.yml
```

Ini akan mengonfigurasi `node1` untuk Backend (beserta database) dan `node2` untuk Frontend.

## 2. Verifikasi Praktikan 2 (Backend)
Setelah ansible selesai tanpa error, jalankan tes backend menggunakan perintah `curl` dari dalam WSL:

**Health Check Backend:**
```bash
curl http://10.166.149.18:3000/
```
*Output yang diharapkan:* `{"message": "Backend is UP and RUNNING!"}`

**Registrasi User (Test Database):**
```bash
curl -X POST http://10.166.149.18:3000/api/register \
     -H "Content-Type: application/json" \
     -d '{"username": "testuser", "password": "password123"}'
```
*Output yang diharapkan:* `{"message": "Registrasi berhasil! Silakan login."}`

## 3. Verifikasi Praktikan 3 (Frontend)
Untuk frontend, buka browser Anda (Chrome/Edge) dan akses:
```text
http://10.166.149.13:8080/
```

Lakukan skenario berikut di halaman web tersebut:
1. **Register dengan user apapun** (Gunakan form registrasi)
2. **Register lagi dengan user sama** (Akan muncul error *username sudah digunakan*)
3. **Login dengan salah password** (Akan muncul pesan *Password salah*)
4. **Login berhasil** (Masukkan username dan password yang benar, akan muncul *Login berhasil!* beserta Token JWT)

> **Catatan Virtualisasi (WSL & Multipass):** Karena *Double Layer Virtualization*, website tidak bisa dibuka langsung melalui IP Multipass di browser Windows. 
> 
> **Jalan Keluarnya (Port Forwarding):**
> Kita harus melakukan *Port Forwarding* dari WSL ke Windows. 
> 
> Buka 2 tab/jendela terminal WSL baru, lalu jalankan dua perintah SSH ini (biarkan terbuka):
> 
> **Terminal 1 (Forwarding Backend):**
> ```bash
> ssh -L 3000:localhost:3000 ubuntu@10.166.149.18 -i ~/.ssh/id_rsa
> ```
> 
> **Terminal 2 (Forwarding Frontend):**
> ```bash
> ssh -L 8080:localhost:8080 ubuntu@10.166.149.13 -i ~/.ssh/id_rsa
> ```
> 
> Setelah itu, Anda bisa membuka browser di Windows dan mengakses:
> ```text
> http://localhost:8080/
> ```
> 
> *(Catatan: Konfigurasi Ansible sudah saya ubah agar Frontend mengarah ke `http://localhost:3000` khusus untuk trik Port Forwarding ini, jadi mohon **jalankan ulang perintah `ansible-playbook -i inventory.yml site.yml`** di WSL Anda sebelum ngetes di browser).*
