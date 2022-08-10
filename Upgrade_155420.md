<p align="center">
  <img height="100" height="auto" src="https://user-images.githubusercontent.com/50621007/183283696-d1c4192b-f594-45bb-b589-15a5e57a795c.png">
</p>

# Peningkatan rantai untuk melakukan 15e65e9a364804671425051606fe0be6536452fe
## Peningkatan manual
Setelah rantai mencapai ketinggian pemutakhiran, Anda akan menemukan pesan kesalahan panik berikut:\
`ERR UPGRADE "xxx" DIPERLUKAN pada ketinggian: 155420`
```
sudo systemctl stop strided
cd $HOME && rm -rf stride
git clone https://github.com/Stride-Labs/stride.git && cd stride
git checkout 4ec1b0ca818561cef04f8e6df84069b14399590e
make build
sudo cp $HOME/stride/build/strided /usr/local/bin
sudo systemctl restart strided && journalctl -fu strided -o cat
```

!!! JANGAN UPGRADE SEBELUM CHAIN MENCAPAI BLOK `155420`!!!
