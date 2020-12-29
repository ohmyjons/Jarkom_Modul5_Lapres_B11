## Lapres Praktikum Jaringan Komputer Modul 5

---

Anggota :

1. 05111840000039 - Sitti Chofifah
2. 05111840000139 - Dohan Pranata W.

---

---

Topologi soal

![](imgs\media\image25.jpg)

Langkah 1 : Melakukan subnetting. Disini kami menggunakan teknik VLSM

![](imgs\media\image21.jpg)

Subnet Jumlah IP Netmask

---

A1 201 /24
A2 2 /30
A3 2 /30
A4 211 /24
A5 3 /29
Total 419 /22

![](imgs\media\image19.png)

Subnet Jumlah IP Length NID Netmask Broadcast Address

---

A1 201 /24 192.168.2.0 255.255.255.0 192.168.2.255
A2 2 /30 192.168.0.4 255.255.255.252 192.168.0.7
A3 2 /30 192.168.0.0 255.255.255.252 192.168.0.3
A4 211 /24 192.168.1.0 255.255.255.0 192.168.1.255
A5 3 /29 192.168.0.8 255.255.255.248 192.168.0.15

Membuat topologi.sh

\# Switch

uml_switch -unix switch1 \> /dev/null \< /dev/null &

uml_switch -unix switch2 \> /dev/null \< /dev/null &

uml_switch -unix switch3 \> /dev/null \< /dev/null &

uml_switch -unix switch4 \> /dev/null \< /dev/null &

uml_switch -unix switch5 \> /dev/null \< /dev/null &

uml_switch -unix switch6 \> /dev/null \< /dev/null &

\# Router

xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA
eth0=tuntap,,,10.151.74.49 eth1=daemon,,,switch1 eth2=daemon,,,switch2
mem=96M &

xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch1
eth1=daemon,,,switch3 eth2=daemon,,,switch4 mem=96M &

xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI
eth0=daemon,,,switch2 eth1=daemon,,,switch5 eth2=daemon,,,switch6
mem=96M &

\# Server

xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG
eth0=daemon,,,switch3 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO
eth0=daemon,,,switch3 mem=128M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO
eth0=daemon,,,switch5 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN
eth0=daemon,,,switch5 mem=128M &

\# Klien
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO
eth0=daemon,,,switch4 mem=96M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK
eth0=daemon,,,switch6 mem=96M &

Setting IP
SURABAYA

nano /etc/network/interfaces
auto eth0
iface eth0 inet static
address 10.151.74.50 = IP_eth0_Surabaya_tiap_kelompok
netmask 255.255.255.252
gateway 10.151.74.49 = IP_tuntap_tiap_kelompok

auto eth1 (A2)
iface eth1 inet static
address 192.168.0.5 = A2
netmask 255.255.255.252

auto eth2 (A3)
iface eth2 inet static
address 192.168.0.1 = A3
netmask 255.255.255.252

![](imgs\media\image26.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

BATU

auto eth0 (A2)
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

auto eth1 (DMZ) (Batu+1-Malang+2-Mojokerto+3)
iface eth1 inet static
address 10.151.73.97
netmask 255.255.255.248

auto eth2 (A1)(Batu-Sidoarjo)
iface eth2 inet static
address 192.168.2.1
netmask 255.255.255.0

![](imgs\media\image23.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Kediri

auto eth0 (Kediri-Surabaya) (A3)
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.252
gateway 192.168.0.1

auto eth1 (Kediri- Probolinggo-Madiun) A5
iface eth1 inet static
address 192.168.0.9
netmask 255.255.255.248

auto eth2 (kediri -gresik) A4
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.0

![](imgs\media\image17.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

MALANG

auto eth0
iface eth0 inet static
address 10.151.83.98 = NID DMZ_tiap_kelompok +2
netmask 255.255.255.248
gateway 10.151.83.97

![](imgs\media\image22.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

MOJOKERTO

auto eth0
iface eth0 inet static
address 10.151.83.99
netmask 255.255.255.248
gateway 10.151.83.97

![](imgs\media\image14.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

PROBOLINGGO

auto eth0 (A5)
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.248
gateway 192.168.0.9

![](imgs\media\image10.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

MADIUN

auto eth0
iface eth0 inet static
address 192.168.0.11
netmask 255.255.255.248
gateway 192.168.0.9

![](imgs\media\image5.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

SIDOARJO

auto eth0
iface eth0 inet dhcp

![](imgs\media\image12.png)

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

GRESIK

auto eth0
iface eth0 inet dhcp

![](imgs\media\image18.png)

Routing

SURABAYA :

DMZ Malang
route add -net 10.151.83.96 netmask 255.255.255.248 gw 192.168.0.6

A1
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.0.6

A4
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.0.2

A5
route add -net 192.168.0.8 netmask 255.255.255.248 gw 192.168.0.2

![](imgs\media\image20.png)

KEDIRI

Default
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.1

BATU

Default
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5

ATUR DHCP
subnet 10.151.83.96 netmask 255.255.255.248 {

}
subnet 192.168.1.0 netmask 255.255.255.0 {

range 192.168.1.2 192.168.1.202;
option routers 192.168.1.1;
option broadcast-address 192.168.1.255;
option domain-name-servers 10.151.73.96;
default-lease-time 600;
max-lease-time 7200;

}

subnet 192.168.2.0 netmask 255.255.255.0 {

range 192.168.2.2 192.168.2.212;
option routers 192.168.2.1;
option broadcast-address 192.168.2.255;
option domain-name-servers 10.151.73.96;
default-lease-time 600;
max-lease-time 7200;

}

Install DHCP relay di kediri dan batu : apt-get install isc-dhcp-relay.

Masukkan IP mojo

Masukkan interface yg di minta

- Kediri : eth2

- Batu : eth2

---

## SOAL

No.1 Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.

![](imgs\media\image24.png)

![](imgs\media\image3.png)

No2.Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.

Instal openssh-server di surabaya malang mojo terlebih dahulu

![](imgs\media\image7.png)

![](imgs\media\image2.png)

lakukan bash no2.sh

Hasil :

![](imgs\media\image9.png)

No.3 Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.

inputkan `iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP` di malang dan mojokerto

![](imgs\media\image1.png)

![](imgs\media\image6.png)

No.4 Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat.

Iptables di malang

iptables -A INPUT -s 192.168.1.0/24 -m time \--timestart 07:00
\--timestop 17:00 \--weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -A INPUT -s 192.168.1.0/24 -j REJECT

![](imgs\media\image4.png)

![](imgs\media\image16.png)

No.5

> iptables -A INPUT -s 192.168.2.0/24 -m time \--timestart 17:00
> \--timestop 23:59 -j ACCEPT
>
> iptables -A INPUT -s 192.168.2.0/24 -m time \--timestart 00:00
> \--timestop 07:00 -j ACCEPT
>
> iptables -A INPUT -s 192.168.2.0/24 -j REJECT

![](imgs\media\image8.png)

7.Terdapat log untuk uml yang melakukan drop

Jadi ketika terjadi DROP maka log akan ter wirte di UML

LOG DROP pada nomer 2 -\>

![](imgs\media\image11.png)

LOG DROPpada nomor soal 3 -\>

Inputkan `iptables -A INPUT -p icmp -m connlimit \--connlimit-above 3 \--connlimit-mask 0 -j LOG \--log-prefix "INPUT(NO3):DROPED: "` pada mojo dan malang
![](imgs\media\image15.png)

![](imgs\media\image13.png)
