# Kelompok_09_KJK_Quiz_1  
| Nama    | NRP                     |
| --------| ------------------------|
| Sean Arthur Tamajaya | 5027251050 |
| Mahrinza Redouane Z. | 5027251074 |

## 1. Finding the IP address of the Attacker and Internal Server

Dalam tahap pengintaian, seorang penyerang umumnya melakukan port scanning (seperti TCP SYN Scan) untuk memetakan titik masuk ke target. Nah, karena hal tersebut IP dari penyerang akan terlihat agresif mengirimkan banyak packet sedangkang IP penerima hanya membalas packet tersebut sesekali. Sehingga untuk mencari IP address dari penyerang kita perlu mencari IP dengan pengiriman packet terbanyak.

Untuk mencari IP address dari attacker saya membuka menu statistics dan memilih conversations, setelah itu saya membuka tab IPv4. Dari situ kita bisa melihat berapa IP address yang paling banyak mengirim kan packet dengan melihat jumlah bytes nya.  

<img width="680" alt="image" src="https://github.com/user-attachments/assets/c5729197-0e9f-42e5-9679-5a6cde0f8cd5" />

Dari screenshot tersebut IP attacker nya bisa terlihat yaitu Adress A yang paling banyak mengirimkan jumlah packets ke B yaitu 2,197 sehingga 192.168.174.137 merupakan IP dari attacker nya dan 192.168.174.1 merupakan IP dari Internal Server nya.

## 2. Finding the open non standard TCP Port

Gunakan filter `tcp.flags.syn == 1 && tcp.flags.ack == 1` untuk mencari paket TCPnya. Ditemukan bahwa server 192.168.174.1 mengirim paket dari port non standard ==2323== ke host 192.168.174.137.

<img width="1349" height="234" alt="2_2" src="https://github.com/user-attachments/assets/a2076e9a-fe21-4491-b358-5303b3c7f92a" />

source: https://classroom.its.ac.id/course/view.php?id=19818 [MyITS Classroom KJK Week-2] 

## 3. Finding who was the first person that successfully login 

Ketika sebuah layanan (seperti Telnet, Http, atau custom listener di port non-standar) tidak dibungkus dengan enkripsi seperti TLS/SSL, semua pertukaran data dikirim dalam bentuk teks mentah. Sehingga ketika lalu lintas ini terekam di file PCAP, analisis atau penyerang dapat merekonstruksi urutan paketnya menjadi sebuah TCP Stream. Hasilnya, muatan data termasuk username dan password dapat dibaca secara telanjang bulat.

Untuk memulai saya mencoba mencari packet dengan port yang unik, dengan membuka menu statistic dan memilih conversations. Disitu saya melihat di tab TCP dan saya memilih packet dengan jumlah pengiriman terbesar, setelah itu saya melakukan filter dengan menggunakan packet temuan saya tersebut. Filter yang saya dapat adalah sebagai berikut:

`ip.addr==192.168.174.137 && tcp.port==64030 && ip.addr==192.168.174.1 && tcp.port==2323`

Nah dari situ saya dapat port dengan port unik yaitu 2323, lalu saya memfollow salah satu dari packet tersebut yang memiliki len=39 dan menemukan hasil ini.

<img width="680" alt="image" src="https://github.com/user-attachments/assets/42aaff7b-9a98-4d46-9fe8-961b04b53018" />

Untuk membuktikan bahwa itu merupakan login pertama kita bisa melakukan filter string "Login Berhasil!" dan melihat waktu dari packet yang keluar, dan disini yang saya temukan adalah port 2323 tersebut yang pertama kali muncul.

<img width="680" alt="image" src="https://github.com/user-attachments/assets/d5ce401e-310f-45a5-bc22-76cad279ff3b" />

## 4 Finding the date of an user login packet

Untuk mencari paket login user Mizu adalah dengan cara memasang filter `tcp contains "Mizu"`. Untuk memastikan bahwa itu adalah paket login Mizu, bisa dengan cara Follow -> TCP Stream. Dari situ terlihat bahwa terdapat user Mizu, passwordnya, dan respons server login berhasil. Untuk mencari tanggal user login, cukup lihat pada tabel di kolom kedua, yaitu kolom time.
<img width="1266" height="247" alt="4_1" src="https://github.com/user-attachments/assets/fbdc755e-8001-4172-b508-845a94c004c3" />

source: https://www.reddit.com/r/wireshark/comments/ycjdmo/contains_keyword_doesnt_work/

## 5. What password wa used to login as mizu

Dari soal no 3, kita bisa melihat bahwa password yang digunakan untuk mizu adalah: batam.

## 6. Finding the attack that was performed by the attacker after the succussful login

Bisa dengan cara memasukkan filter `arp.opcode == 2`. ditemukan banyak sekali Gratuitous ARP Reply yang mengklaim alamat IP 192.168.174.1, serta muncul peringatan "duplicate use of 192.168.174.1 detected". Ini adalah tanda dari serangan ARP Spoofing/ARP Poisoning.

<img width="1471" height="536" alt="6_1" src="https://github.com/user-attachments/assets/fb68be87-9a18-431f-9871-c0f3b7d576e0" />

source: https://oneuptime.com/blog/post/2026-03-20-wireshark-detect-arp-spoofing/view

## 7. Finding the second person to succesfully login and gain administrative accsess

Kredensial kedua ini berhubungan dengan eksploitasi pasca-akses. Setelah memanipulasi alur jaringan internal, attacker menempatkan dirinya di tengah-tengah komunikasi. Ketika administrator berusaha login di ke layanan cleartext yang telah dimanipulasi tersebut attacker bisa melihat password dengan akses hak tinggi. 

Di soal no 7 ini, saya menggunakan cara yang sama dengan soal no 3, jadi saya menekan Ctrl + F dan menggunakan pengaturan display string dan Packet Bytes, setelah itu saya memfilter dengan kata "Login Berhasil!" dan menekan find dua kali karena yang pertama pasti upaya login dari mizu, ini hasil yang saya temukan:  

<img width="680" alt="image" src="https://github.com/user-attachments/assets/2975231f-3c2e-4387-afb1-549a92b8126a" />

## 9 What password was used to gaining admin accsess

Dari soal no 7, kita bisa melihat bahwa password yang digunakan untuk mengakses admin adalah: kantor123.



