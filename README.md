# Kelompok_09_KJK_Quiz_1  
| Nama    | NRP                     |
| --------| ------------------------|
| Sean Arthur Tamajaya | 5027251050 |
| Mahrinza Redouane Z. | 5027251074 |

## 1. Finding the IP address of the Attacker and Internal Server

Untuk mencari IP address dari attacker saya membuka menu statistics dan memilih conversations, setelah itu saya membuka tab IPv4. Dari situ kita bisa melihat berapa IP address yang paling banyak mengirim kan packet dengan melihat jumlah bytes nya.  

<img width="680" alt="image" src="https://github.com/user-attachments/assets/c5729197-0e9f-42e5-9679-5a6cde0f8cd5" />

Dari screenshot tersebut IP attacker nya bisa terlihat yaitu Adress A yang paling banyak mengirimkan jumlah packets ke B yaitu 2,197 sehingga 192.168.174.137 merupakan IP dari attacker nya dan 192.168.174.1 merupakan IP dari Internal Server nya.

## 3. Finding who was the first person that successfully login 

Di soal 3 ini saya berusaha mencari packet dengan port yang unik, dengan membuka menu statistic dan memilih conversations. Disitu saya melihat di tab TCP dan saya memilih packet dengan jumlah pengiriman terbesar, setelah itu saya melakukan filter dengan menggunakan packet temuan saya tersebut. Filter yang saya dapat adalah sebagai berikut:

`ip.addr==192.168.174.137 && tcp.port==64030 && ip.addr==192.168.174.1 && tcp.port==2323`

Nah dari situ saya dapat port dengan port unik yaitu 2323, lalu saya memfollow salah satu dari packet tersebut yang memiliki len=39 dan menemukan hasil ini.

<img width="680" alt="image" src="https://github.com/user-attachments/assets/42aaff7b-9a98-4d46-9fe8-961b04b53018" />

Untuk membuktikan bahwa itu merupakan login pertama kita bisa melakukan filter string "Login Berhasil!" dan melihat waktu dari packet yang keluar, dan disini yang saya temukan adalah port 2323 tersebut.

<img width="680" alt="image" src="https://github.com/user-attachments/assets/d5ce401e-310f-45a5-bc22-76cad279ff3b" />

## 5. What password wa used to login as mizu

Dari soal no 3, kita bisa melihat bahwa password yang digunakan untuk mizu adalah: batam.

## 7. Finding the second person to succesfully login and gain administrative accsess

Di soal no 7 ini, saya menggunakan cara yang sama dengan soal no 3, jadi saya menekan Ctrl + F dan menggunakan pengaturan display string dan Packet Bytes, setelah itu saya memfilter dengan kata "Login Berhasil!" dan menekan find dua kali karena yang pertama pasti upaya login dari mizu, ini hasil yang saya temukan:  

<img width="680" alt="image" src="https://github.com/user-attachments/assets/2975231f-3c2e-4387-afb1-549a92b8126a" />

## 9 What password was used to gaining admin accsess

Dari soal no 7, kita bisa melihat bahwa password yang digunakan untuk mengakses admin adalah: kantor123.



