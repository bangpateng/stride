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
- RAM 32GB
- Penyimpanan 200GB (SSD atau NVME)
- Koneksi Internet permanen (lalu lintas akan minimal selama testnet; 10Mbps akan banyak - untuk produksi diharapkan setidaknya 100Mbps)

## 1. Jalankan Node (Script Otomatis)
Jangan Lupa Masukan atau Ketikan dan Buat Nama Validator Anda
```
wget -O stride.sh https://raw.githubusercontent.com/bangpateng/stride/main/stride.sh && chmod +x stride.sh && ./stride.sh
```
## 2. Load Sistem
```
source $HOME/.bash_profile
```
## 3. Membuat Wallet
Jangan Lupa Simpan Address & Mnemonic Anda
```
strided keys add $WALLET
```
(Opsional) Gunakan Peritah di Bawah Jika Anda Memiliki Wallet Lama
```
strided keys add $WALLET --recover
```
## 4. Masukan Variable 2 (Untuk Simpan Informasi Wallet)
```
STRIDE_WALLET_ADDRESS=$(strided keys show $WALLET -a)
STRIDE_VALOPER_ADDRESS=$(strided keys show $WALLET --bech val -a)
echo 'export STRIDE_WALLET_ADDRESS='${STRIDE_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export STRIDE_VALOPER_ADDRESS='${STRIDE_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```
## 5. Jalankan Sync Log

<p align="center">
  <img height="200" height="auto" src="https://user-images.githubusercontent.com/38981255/183641338-6fab957b-06c7-4542-ad4b-a030d73f13e4.PNG">
</p>

```
journalctl -fu strided -o cat
```
## 6. Claim Token Faucet (Via Discord)
Join Discord Cari Server #token-faucet : 

Masukan Perintah di Bawah :
```
$faucet-stride:<STRIDE_WALLET_ADDRESS>
```
Untuk Membuat Validator, Anda Harus Menunggu Hingga Status Sinkronisasi Node Kalian Berstatus "FALSE" dan Tunggu Hingga Token Faucet Anda Sudah Tiba, Gunakan Perintah di Bawah :

- Check Saldo
```
strided query bank balances $STRIDE_WALLET_ADDRESS
```
<p align="center">
  <img height="200" height="auto" src="https://user-images.githubusercontent.com/38981255/183638647-998687d0-6d87-48da-89f5-e4daa737ccc9.PNG">
</p>

- Check Status Sinkronisasi

```
strided status 2>&1 | jq .SyncInfo
```
<p align="center">
  <img height="200" height="auto" src="https://user-images.githubusercontent.com/38981255/183639084-09757f91-a64b-4292-89b7-c09286e4b521.png">
</p>

## 7. Membuat Validator
(Pastikan Status Sudah FALSE)

<p align="center">
  <img height="200" height="auto" src="https://user-images.githubusercontent.com/38981255/183640037-dd39bf19-9484-445c-bc23-6f3d133356f6.PNG">
</p>

```
strided tx staking create-validator \
  --amount 10000000ustrd \
  --from $WALLET \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(strided tendermint show-validator) \
  --moniker $NODENAME \
  --chain-id $STRIDE_CHAIN_ID
```

## 8. Edit Profil Validator
Anda Cukup Mengubah <your_keybase_id> , <your_website> , <your_validator_description> Untuk Yang Lain Biarkan Optional Bawaan

<p align="center">
  <img height="200" height="auto" src="https://user-images.githubusercontent.com/38981255/183640057-3cb24589-fe92-407c-80c0-9b843efb736f.png">
</p>

```
strided tx staking edit-validator \
  --moniker=$NODENAME \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=$STRIDE_CHAIN_ID \
  --from=$WALLET
```
