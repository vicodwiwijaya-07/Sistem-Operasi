<h4>Nama : Vico Dwi Wijaya<h4>
<h4>NIM  : 254107020259<h4>
<h4>Kelas: TI-1H<h4>

## Percobaan 1 : Direktory

1. Melihat direktori HOME 
```
$ pwd  
$ echo $HOME 
```
![alt text](image/image.png)
2. Melihat direktori aktual dan parent direktori 
```
$ pwd 
$ cd . 
$ pwd 
$ cd .. 
$ pwd 
$ cd 
```
![alt text](image/image-1.png)
 
3. Membuat satu direktori, lebih dari satu direktori atau sub direktori
``` 
$ pwd 
$ mkdir A B C A/D A/E B/F A/D/A 
$ ls -l 
$ ls -l A 
$ ls -l A/D 
```
![alt text](image/image-2.png)
 
4. Menghap us  satu  atau  lebih  direktori  hanya  dapat  dilakukan  pada  direktori 
kosong  dan  hanya  dapat  dihapus  oleh  pemiliknya  kecuali  bila  diberikan  ijin 
aksesnya 
```
$ rmdir B 
$ ls -l B 
$ rmdir B/F B 
$ ls -l B  
```
![alt text](image/image-3.png)
 
5. Navigasi direktori dengan instruksi cd  untuk  pindah  dari  satu  direktori  ke 
direktori  lain. 
```
$ pwd 
$ ls -l 
$ cd A 
$ pwd 
$ cd .. 
$ pwd 
$ cd /home/<user>/C 
$ pwd 
$ cd /<user/C 
$ pwd 
```
![alt text](image/image-4.png)

## Percobaan 2 : Manipulasi file 

1. Perintah  cp  untuk  mengkopi file atau seluruh direktori 
```
$ cat > contoh  
Membuat sebuah file 
[Ctrl-d] 
$ cp contoh contoh1 
$ ls -l  
$ cp contoh A 
$ ls –l A 
$ cp contoh contoh1 A/D 
$ ls –l A/D 
```
![alt text](image5.png)
 
2. Perintah  mv untuk memindah file 
``` 
$ mv contoh contoh2 
$ ls -l 
$ mv contoh1 contoh2 A/D 
$ ls –l A/D 
$ mv contoh contoh1 C 
$ ls –l C 
```
![alt text](image6.png)
 
3. Perintah rm untuk menghapus file 
```
$ rm contoh2 
$ ls -l 
$ rm –i contoh 
$ rm –rf A C 
$ ls -l 
```
![alt text](image7.png)
## Percobaan 3 : Symbolic Link

1. Membuat shortcut (file link) 
```
$ echo "Hallo apa khabar" > halo.txt 
$ ls -l 
$ ln halo.txt z 
$ ls -l 
$ cat z 
$ mkdir mydir 
$ ln z mydir/halo.juga 
$ cat mydir/halo.juga 
$ ln -s z bye.txt 
$ ls -l bye.txt 
$ cat bye.txt 
```
![alt text](image.png)
## Percobaan 4 : Melihat Isi File 
```
$ ls –l 
$ file halo.txt 
$ file bye.txt
```
![alt text](image2.png)
## Percobaan 5 : Mencari file

1. Perintah  find 
```
$ find /home –name “*.txt” –print > myerror.txt 
$ cat myerror.txt 
$ find . –name “*.txt” –exec wc –l ‘{}’ ‘;’ 
```
2. Perintah  which
``` 
$ which ls
``` 
3. Perintah  locate 
```
$ locate “*.txt”
```
![alt text](image-1.png)

## Percobaan 6 : Mencari text pada file
```
$ grep Hallo *.txt 
```
![alt text](image-2.png)

## LATIHAN: 
1. Cobalah urutan perintah berikut : 
```
$ cd 
$ pwd 
$ ls –al 
$ cd . 
$ pwd 
$ cd .. 
$ pwd 
$ ls -al 
$ cd .. 
$ pwd 
$ ls -al 
$ cd /etc 
$ ls –al | more 
$ cat passwd 
$ cd – 
$ pwd
```
![alt text](image2/image-3.png)
![alt text](<image2/Screenshot 2026-03-12 040525.png>)
![alt text](<image2/Screenshot 2026-03-12 040830.png>)
2. Lanjutkan  penelusuran  pohon  pada  sistem  file  menggunakan  cd, ls,  pwd dan cat. 
Telusuri direktory  /bin, /usr/bin, /sbin, /tmp dan  /boot. 

```
cd /bin && ls
```
![alt text](image2/image-4.png)

```
cd /usr/bin && ls
```
![alt text](image2/image-5.png)

```
cd /sbin && ls
```
![alt text](image2/image-6.png)

```
cd /tmp && ls
```
![alt text](image2/image-7.png)

```
cd /boot && ls
```
![alt text](image2/image-8.png)

3. Telusuri direktory /dev.  I dentifikasi perangkat yang tersedia. Identifikasi tty 
(termninal) Anda (ketik who am i); siapa pemilih tty Anda (gunakan ls –l). 
```
who am i
# hasilnya: user tty1
ls -l /dev/tty1
```
![alt text](image2/image-9.png)

4. Telusuri derectory /proc.  Tampilkan isi file  interrupts,  devices, 
cpuinfo, meminfo  dan uptime menggunakan  perintah cat.    Dapatkah  Anda melihat mengapa directory  /proc disebut  pseudo -filesystem  yang memungkinkan 
akses ke struktur data kernel ?
```
cd /proc
cat interrupts
cat devices
cat cpuinfo
cat meminfo
cat uptime
```
![alt text](image2/image-10.png)
![alt text](image2/image-11.png)
![alt text](image2/image-12.png)
![alt text](image2/image-13.png)
![alt text](image2/image-14.png)

### Kenapa disebut Pseudo-filesystem? 
Karena file-file di dalam /proc sebenarnya tidak ada di harddisk. Mereka adalah data langsung dari RAM yang disediakan oleh Kernel Linux untuk menunjukkan status sistem secara real-time.

5. Ubahlah direktory home ke user lain secara langsung menggunakan  cd ~username.
```
cd ~nama_user_lain 
```
![alt text](image2/image-15.png)

6. Ubah kembali ke direktory home Anda. 
```
cd ~
```
![alt text](image2/image-16.png)

7. Buat subdirektory  work dan play. 
![alt text](image2/image-17.png)
8. Hapus subdirektory work. 
![alt text](image2/image-18.png)

9. Copy file /etc/passwd ke direktory home Anda. 
![alt text](image2/image-19.png)
10. Pindahkan ke subirectory  play. 
![alt text](image2/image-20.png)

11. Ubahlah ke subdirektory play dan buat symbolic link dengan nama terminal yang 
menunjuk ke perangkat tty.  Apa  yang  terjadi  jika  melakukan  hard link ke perangkat 
tty ? 
```
ln -s /dev/tty terminal
```
![alt text](image2/image-21.png)
### Apa yang terjadi jika Hard Link ke tty?
gagal (Error). Sistem Linux tidak mengizinkan hard link ke file perangkat (device files) atau antar file system yang berbeda.

12. Buatlah file bernama  hello.txt yang berisi kata ”hello word”.  Dapatkah Anda 
gunakan  ”cp”  menggunakan  ”terminal”  sebagai  file  asal  untuk  menghasilkan  efek yang sama ? 
![alt text](image2/image-22.png)
Jika menggunakan cp terminal hello.txt: Terminal akan "menunggu" kamu mengetik sesuatu. Apa pun yang ketik di layar akan masuk ke file hello.txt
13. Copy  hello.txt ke terminal.   Apa  yang  terjadi ? 
```
cp hello.txt /dev/tty
```
![alt text](image2/image-23.png)

file yang kita tadi kita ketik akan muncul

14. Masih  direktory  home,  copy  keseluruhan  direktory  play  ke  direktory  bernama  work
![alt text](image2/image-24.png) 
menggunakan symbolic link. 
15. Hapus direktory work dan isinya dengan satu perintah
![alt text](image2/image-25.png) 