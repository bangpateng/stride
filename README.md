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

### Periksa kunci validator Anda
```
[[ $(strided q staking validator $STRIDE_VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(strided status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"

```
### Dapatkan daftar validator
```
strided q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

## Dapatkan daftar rekan yang saat ini terhubung dengan id
```
curl -sS http://localhost:${STRIDE_PORT}657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

## Perintah yang berguna
### Manajemen Pelayanan
Periksa log
```
journalctl -fu strided -o cat
```

Start service
```
sudo systemctl start strided
```

Stop service
```
sudo systemctl stop strided
```

Restart service
```
sudo systemctl restart strided
```

### Info simpul (status)
Informasi sinkronisasi
```
strided status 2>&1 | jq .SyncInfo
```

Validator info
```
strided status 2>&1 | jq .ValidatorInfo
```

Node info
```
strided status 2>&1 | jq .NodeInfo
```

Show node id
```
strided tendermint show-node-id
```

### Perintah Operasi dompet
List of wallets
```
strided keys list
```

Recover wallet
```
strided keys add $WALLET --recover
```

Delete wallet
```
strided keys delete $WALLET
```

Get wallet balance
```
strided query bank balances $STRIDE_WALLET_ADDRESS
```

Transfer funds
```
strided tx bank send $STRIDE_WALLET_ADDRESS <TO_STRIDE_WALLET_ADDRESS> 10000000ustrd
```

### Voting
```
strided tx gov vote 1 yes --from $WALLET --chain-id=$STRIDE_CHAIN_ID
```

### Staking, Delegasi, dan Hadiah
Delegate stake
```
strided tx staking delegate $STRIDE_VALOPER_ADDRESS 10000000ustrd --from=$WALLET --chain-id=$STRIDE_CHAIN_ID --gas=auto
```

Redelegate stake from validator to another validator
```
strided tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 10000000ustrd --from=$WALLET --chain-id=$STRIDE_CHAIN_ID --gas=auto
```

Withdraw all rewards
```
strided tx distribution withdraw-all-rewards --from=$WALLET --chain-id=$STRIDE_CHAIN_ID --gas=auto
```

Withdraw rewards with commision
```
strided tx distribution withdraw-rewards $STRIDE_VALOPER_ADDRESS --from=$WALLET --commission --chain-id=$STRIDE_CHAIN_ID
```

### Manajemen validator
Edit Profil validator
```
strided tx staking edit-validator \
  --moniker=$NODENAME \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=$STRIDE_CHAIN_ID \
  --from=$WALLET
```

Unjail validator
```
strided tx slashing unjail \
  --broadcast-mode=block \
  --from=$WALLET \
  --chain-id=$STRIDE_CHAIN_ID \
  --gas=auto
```

### Hapus Node/Simpul
Perintah ini akan sepenuhnya menghapus node dari server. Gunakan dengan risiko Anda sendiri!
```
sudo systemctl stop strided
sudo systemctl disable strided
sudo rm /etc/systemd/system/stride* -rf
sudo rm $(which strided) -rf
sudo rm $HOME/.stride* -rf
sudo rm $HOME/stride -rf
sed -i '/STRIDE_/d' ~/.bash_profile
```

### Pruning for state sync node
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="2000"
pruning_interval="50"
snapshot_interval="2000"
snapshot_keep_recent="5"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.stride/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.stride/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.stride/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.stride/config/app.toml
sed -i -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" $HOME/.stride/config/app.toml
sed -i -e "s/^snapshot-keep-recent *=.*/snapshot-keep-recent = \"$snapshot_keep_recent\"/" $HOME/.stride/config/app.toml
```
