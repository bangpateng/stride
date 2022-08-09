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

## Siapkan Full Node Anda
### Gunakan (Script Otomatis)
Anda dapat mengatur langkah penuh Anda dalam beberapa menit dengan menggunakan skrip otomatis di bawah ini. Ini akan meminta Anda untuk memasukkan nama node validator Anda!
```
wget -O stride.sh https://raw.githubusercontent.com/bangpateng/stride/main/stride.sh && chmod +x stride.sh && ./stride.sh
```

## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```

Next you have to make sure your validator is syncing blocks. You can use command below to check synchronization status
```
strided status 2>&1 | jq .SyncInfo
```

### Membuat Wallet
To create new wallet you can use command below. Don’t forget to save the mnemonic
```
strided keys add $WALLET
```

(OPSIONAL) Untuk memulihkan dompet Anda menggunakan frase seed
```
strided keys add $WALLET --recover
```

Untuk mendapatkan daftar dompet saat ini
```
strided keys list
```

### Simpan info dompet (Variable)
Add wallet and valoper address into variables 
```
STRIDE_WALLET_ADDRESS=$(strided keys show $WALLET -a)
STRIDE_VALOPER_ADDRESS=$(strided keys show $WALLET --bech val -a)
echo 'export STRIDE_WALLET_ADDRESS='${STRIDE_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export STRIDE_VALOPER_ADDRESS='${STRIDE_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Danai dompet Anda | Claim Faucet
Untuk membuat validator terlebih dahulu, Anda perlu mendanai dompet Anda dengan token testnet.
Untuk isi ulang dompet Anda, gabung [Stride discord server](https://discord.gg/n6KrK77t) dan navigasikan ke:
- **#token-faucet** untuk meminta token uji

Perintah Untuk meminta faucet di Server Discord:
```
$faucet-stride:<STRIDE_WALLET_ADDRESS>
```

### Membuat Validator
Sebelum membuat validator, pastikan Anda memiliki setidaknya 1 strd (1 strd sama dengan 1000000 ustrd) dan node Anda tersinkronisasi

Untuk memeriksa saldo dompet Anda:
```
strided query bank balances $STRIDE_WALLET_ADDRESS
```
> Jika dompet Anda tidak menunjukkan saldo apa pun, kemungkinan simpul Anda masih disinkronkan. Silahkan tunggu sampai selesai untuk sinkronisasi lalu lanjutkan

Untuk membuat perintah jalankan validator Anda di bawah ini
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

