# RAFFLES
UTS_JARKOM2
🏫 Universitas Teknologi Bandung
Mata Kuliah: Jaringan Komputer 2
Dosen Pengampu: Muchamad Rusdan, S.T., M.T.
Kelas: TIF K 23B
Nama: Raffles Figo Tristan Siahaan
NIM: 22552011287

📋 Deskripsi Proyek
Repositori ini berisi konfigurasi lengkap untuk implementasi:

🔄 DHCP Server (menggunakan isc-dhcp-server)

🌐 DNS Server (menggunakan bind9)

🔥 Firewall (menggunakan iptables)

Seluruh konfigurasi dijalankan di Ubuntu Server 20.04 dalam lingkungan virtual menggunakan VirtualBox/WSL/VMware.

🛠️ Struktur Direktori
graphql
Salin
Edit
.
├── dhcpd.conf                # Konfigurasi DHCP
├── named.conf.local          # Konfigurasi zona DNS
├── db.server.local           # Zona forward DNS (server.local)
├── db.192                    # Zona reverse DNS
├── iptables.rules            # Aturan firewall
├── iptables.sh               # (Opsional) Script firewall
└── README.md                 # Dokumentasi proyek ini
⚙️ Konfigurasi DHCP (/etc/dhcp/dhcpd.conf)
conf
Salin
Edit
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.10 192.168.10.100;
  option routers 192.168.10.1;
  option domain-name-servers 192.168.10.1;
}
🌐 Konfigurasi DNS (/etc/bind)
named.conf.local

conf
Salin
Edit
zone "server.local" {
    type master;
    file "/etc/bind/db.server.local";
};

zone "10.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192";
}
db.server.local

conf
Salin
Edit
@       IN      NS      server.local.
@       IN      A       192.168.10.1
db.192

conf
Salin
Edit
@       IN      NS      server.local.
1       IN      PTR     server.local.
🔥 Konfigurasi Firewall (iptables)
bash
Salin
Edit
# Blok ping dari luar subnet
iptables -A INPUT -p icmp --icmp-type echo-request ! -s 192.168.10.0/24 -j DROP

# Izinkan SSH, HTTP, HTTPS
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
