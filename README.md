# Welcome To Github Bang Pateng | Bang Pateng Is Indonesia's Cryptocurrency Community, Since 2017. We Discuss Airdrop, AMA, Trading. Node and More

<p align="center">
  <img height="200" height="auto" src="https://user-images.githubusercontent.com/38981255/183307761-5557fc3d-1ab5-4a21-8a57-99b93b36ed46.png">

<p align="center">
  <img height="100" height="auto" src="https://user-images.githubusercontent.com/50621007/183283696-d1c4192b-f594-45bb-b589-15a5e57a795c.png">
</p>

# Cara Menjalankan Simpul/Node — STRIDE

Dokumentasi resmi:
>- [Petunjuk penyiapan Validator](https://github.com/Stride-Labs/testnet)

Penjelajah:
>- https://stride.explorers.guru

## Persyaratan Perangkat Keras
Seperti rantai Cosmos-SDK lainnya, persyaratan perangkat kerasnya cukup sederhana.

### Persyaratan Perangkat Keras Minimum
- 4x CPU; semakin cepat kecepatan jam semakin baik
  - RAM 8 GB
  - Penyimpanan 100GB (SSD atau NVME)
  - Koneksi Internet permanen (lalu lintas akan minimal selama testnet; 10Mbps akan banyak - untuk produksi diharapkan setidaknya 100Mbps)

### Persyaratan Perangkat Keras yang Direkomendasikan
- 4x CPU; semakin cepat kecepatan jam semakin baik
  -RAM 32GB
  - Penyimpanan 200GB (SSD atau NVME)
  - Koneksi Internet permanen (lalu lintas akan minimal selama testnet; 10Mbps akan banyak - untuk produksi diharapkan setidaknya 100Mbps)

## Jalankan Node (Script Otomatis)
Jangan Lupa Masukan atau Ketikan dan Buat Nama Validator Anda
```
wget -O stride.sh https://raw.githubusercontent.com/bangpateng/stride/main/stride.sh && chmod +x stride.sh && ./stride.sh
```
## Load Sistem
```
source $HOME/.bash_profile
```
## Membuat Wallet
Jangan Lupa Simpan Address & Mnemonic Anda
```
strided keys add $WALLET
```
(Opsional) Gunakan Peritah di Bawah Jika Anda Memiliki Wallet Lama
```
strided keys add $WALLET --recover
```
## Masukan Variable 2 (Untuk Simpan Informasi Wallet)
```
STRIDE_WALLET_ADDRESS=$(strided keys show $WALLET -a)
STRIDE_VALOPER_ADDRESS=$(strided keys show $WALLET --bech val -a)
echo 'export STRIDE_WALLET_ADDRESS='${STRIDE_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export STRIDE_VALOPER_ADDRESS='${STRIDE_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```
