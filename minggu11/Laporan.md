# Laporan Manajemen File & User/Group

<h4>Nama : Vico Dwi Wijaya<h4>
<h4>NIM  : 254107020259<h4>
<h4>Kelas: TI-1H<h4>
<h4>Absen: 28<h4>

## Praktikum 9.1 — Permissions
Langkah 1: Buat direktori kerja dan dua file uji.
```
mkdir ~/lab-permissions && cd ~/lab-permissions
echo "data rahasia" > secret.txt
echo '#!/bin/bash' > myscript.sh
echo 'echo Hello' >> myscript.sh
ls-la
```
Langkah 2: Jadikan secret.txt privat hanya untuk owner.
```
chmod 600 secret.txt
ls-l secret.txt
```
Langkah 3: Jadikan myscript.sh dapat dijalankan.
```
chmod 755 myscript.sh
ls-l myscript.sh
./myscript.sh
```

Langkah 4: Buat direktori bersama dan amati efek SGID sederhana.
```
mkdir shared-dir
chmod g+s shared-dir
ls-ld shared-dir
```
Langkah 5: Uji efek umask pada file baru.
```
umask
umask 027
touch testfile-027
ls-l testfile-027
```
**Analisis**
1. Mengapa secret.txt tidak dapat dibaca oleh group dan others setelah chmod 600?
2. Apa perbedaan arti 600 dan 755 terhadap file yang diuji?
3. Setelah umask 027, permission apa yang dihasilkan untuk file baru, dan mengapa bukan 777?

## Praktikum9.2—ACL
Langkah1: Siapkan file dan lihat permission standar tanpaACL tambahan.
```
mkdir ~/lab-acl && cd ~/lab-acl
echo "Data penting" > confidential.txt
chmod 640 confidential.txt
ls-l confidential.txt
getfacl confidential.txt
```
Langkah2: Beri akses baca kesatu user tertentu tanpa mengubah owner atau group.
```
setfacl-m u:userA:r confidential.txt
ls-l confidential.txt
getfacl confidential.txt
```
Langkah3: Buat direktori bersama yang mewariskan ACL ke file baru.
```
mkdir shared
setfacl-d-m u:userA:rwx shared
setfacl-d-m u:userB:r-x shared
getfacl shared
touch shared/inherited.txt
getfacl shared/inherited.txt
```
**Analisis**
1. Mengapa getfacl confidential.txt awalnya tidak menampilkan user tertentu?
2. Setelah setfacl-m u:userA:r confidential.txt, apa perbedaan output ls-l dan getfacl?
3. Mengapa file inherited.txt mewarisi ACL dari direktori shared?

## Praktikum 9.3A—Membuat dan Mengelola User
```
# buat dua user
sudo useradd-m-s /bin/bash userA
sudo useradd-m-s /bin/bash userB
sudo passwd userA
sudo passwd userB
# verifikasi
id userA
getent passwd userA
# modifikasi shell userA
sudo usermod-s /bin/zsh userA
getent passwd userA
# lock dan unlock userB
sudo usermod-L userB
sudo passwd-S userB
sudo usermod-U userB
sudo passwd-S userB
```
**Pertanyaan:**
1. Apa perbedaan output id userA sebelum dan sesudah menambah group?
2. Bagaimana status passwd-S userB berubah saat akun di-lock?

## Praktikum 9.3B—Group Management
```
# buat dua group
sudo groupadd labgroup
sudo groupadd readonly-group
# tambahkan userA ke kedua group
sudo usermod-aG labgroup,readonly-group userA
# tambahkan userB hanya ke readonly-group
sudo usermod-aG readonly-group userB
# verifikasi
id userA
id userB
getent group labgroup
getent group readonly-group
```
Pertanyaan:
1. Apa yang ditampilkan id user A vs groups userA?
2. Mengapa -a pada user mod-aG penting?

Praktikum 9.3C—Password Aging Policy
```
# set aging policy untuk userA
sudo chage-M 60-W 7-m 1 userA
sudo chage-l userA
# paksa userA ganti password saat login pertama
sudo chage-d 0 userA
# kunci password userB
sudo passwd-l userB
sudo passwd-S userB
# unlock kembali
sudo passwd-u userB
sudo passwd-S userB
```
Pertanyaan:
1. Apa arti nilai yang ditampilkan chage-l userA?
2. Bagaimana cara membuktikan userB terkunci dari output passwd-S?
3. Kapan sebaiknya menggunakan chage-d 0 vs passwd-e?

Praktikum 9.4 — Konfigurasi sudo
**Tujuan**: membuat aturan sudo terbatas, memverifikasi hak akses, dan membaca log.
**Langkah 1**: Buat file konfigurasi sudo khusus untuk userA.
```
sudo visudo-f /etc/sudoers.d/lab-userA
```
 Jika sintaks salah, visudo akan memperingatkan sebelum file disimpan.
Isi file dengan aturan berikut:
```
userA ALL=(root) NOPASSWD: /usr/bin/apt update, /usr/bin/apt
upgrade
userA ALL=(root) /bin/systemctl status *
```
Langkah 2: Verifikasi aturan yang aktif dan uji hasilnya.
```
sudo-l-U userA
sudo grep "userA" /var/log/auth.log | tail-10
```

**Analisis**
1. Mengapa aturan disimpan di /etc/sudoers.d//, bukan langsung di /etc/sudoers?
2. Mana perintah yang bisa dijalankan tanpa password, dan mana yang masih perlu autentikasi?
3. Informasi apa saja yang dicatat di log sudo?

## Praktikum 9.5 — Disk Quota
Tujuan: memahami alur quota secara aman pada loopback filesystem tanpa mengubah filesystem utama.
Langkah 1: Buat image filesystem kecil dan mount dengan opsi quota.
```
sudo dd if=/dev/zero of=/tmp/quota-test.img bs=1M count=100
sudo mkfs.ext4 /tmp/quota-test.img
sudo mkdir-p /mnt/quota-test
sudo mount-o loop,usrquota,grpquota /tmp/quota-test.img /mnt/
quota-test
```
Langkah 2: Buat database quota dan aktifkan enforcement.
```
sudo quotacheck-cug /mnt/quota-test
sudo quotaon-v /mnt/quota-test
sudo repquota /mnt/quota-test
```
Langkah 3: Tetapkan quota untuk user uji dan amati hasilnya.
```
sudo edquota-u userA
# contoh: soft block 5120, hard block 10240
sudo repquota /mnt/quota-test
```
Langkah 4: Bersihkan lingkungan uji setelah selesai.
```
sudo quotaoff /mnt/quota-test
sudo umount /mnt/quota-test
sudo rm /tmp/quota-test.img
```
Analisis
1. Apa perbedaan soft limit dan hard limit saat quota mulai terlampaui?
2. Mengapa praktikum ini memakai loopback filesystem, bukan langsung /home/?
3. Dari output repquota, informasi apa yang menunjukkan quota sudah aktif?

## 1.7 Latihan

### Latihan Latihan 9.A — Audit dan Kolaborasi
1. Temukan file SUID aktif dengan find /-perm-4000-type f 2>/dev/null, lalu jelaskan
tiga file yang Anda kenali beserta alasannya.
2. Cari direktori world-writable dan tentukan mana yang valid dan mana yang berisiko.
3. Rancang konfigurasi permission standar dan ACL untuk direktori proyek /srv/webapp/ agar
group webapp-team dapat menulis, user deploy hanya membaca, dan file baru selalu mewarisi
group proyek.

### Latihan Latihan 9.B — Kebijakan Akun dan Quota
Tuliskan langkah untuk membuat user intern, menambahkannya ke group labgroup, memaksa pergan
tian password tiap 45 hari (warning 7 hari), memberi izin sudo hanya untuk systemctl status, dan
menetapkan quota ruang serta inode sederhana pada /home/.
