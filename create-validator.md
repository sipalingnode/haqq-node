<p style="font-size:14px" align="right">
<a href="https://t.me/Aerdropmobile" target="_blank">Join our telegram <img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
<a href="https://twitter.com/AlldiiR" target="_blank">Join our twitter <img src="https://user-images.githubusercontent.com/108946833/184274157-08210464-fa03-493d-b01c-2420c67a524f.jpg" width="30"/></a>
</p>

<p align="center">
  <img width="100" height="auto" src="https://user-images.githubusercontent.com/38981255/187036471-e23ab080-2e03-46b7-8513-23e1f6612b4a.png">
</p>

# Haqq Testnet Node
## Spek VPS

|  Komponen |  Persyaratan Minimum |
| ------------ | ------------ |
| CPU  | 4 cores (Intel Xeon Skylake or newer) |
| RAM | 32GB RAM  |
| Penyimpanan  | 500GB |

## Install Node
```
wget -O haqq.sh https://raw.githubusercontent.com/xsons/TestnetNode/main/Haqq/haqq.sh && chmod +x haqq.sh && ./haqq.sh
```
Ketika instalasi selesai, silakan muat variabel ke dalam sistem
```
source $HOME/.bash_profile
```

Untuk mempercepat Log bisa gunakan ini
```
sudo systemctl stop haqqd 
haqqd tendermint unsafe-reset-all --home $HOME/.haqqd --keep-addr-book 
pruning="custom" 
pruning_keep_recent="100" 
pruning_keep_every="0" 
pruning_interval="10" 
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.haqqd/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.haqqd/config/app.toml 
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.haqqd/config/app.toml 
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.haqqd/config/app.toml 
cd 
rm -rf ~/.haqqd/data; \ 
wget -O - http://snap.stake-take.com:8000/haqq.tar.gz | tar xf - 
mv $HOME/root/.haqqd/data $HOME/.haqqd 
rm -rf $HOME/root 
wget -O $HOME/.haqqd/config/addrbook.json "https://raw.githubusercontent.com/StakeTake/guidecosmos/main/haqq/haqq_53211-1/addrbook.json" 
sudo systemctl restart haqqd && journalctl -u haqqd -f -o cat
```
**Jika sudah jalan langsung CTRL+C aja bang**

## Import wallet yang sudah dibuat

Untuk memulihkan dompet Anda menggunakan frase seed
```
haqqd keys add $WALLET --recover
```

## Simpan info dompet
Tambahkan dompet dan alamat valoper ke dalam variabel
```
HAQQ_WALLET_ADDRESS=$(haqqd keys show $WALLET -a)
HAQQ_VALOPER_ADDRESS=$(haqqd keys show $WALLET --bech val -a)
echo 'export HAQQ_WALLET_ADDRESS='${HAQQ_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export HAQQ_VALOPER_ADDRESS='${HAQQ_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Claim Faucet haqq
Jalankan Perintah Untuk Mendapatkan Private Key
```
haqqd keys unsafe-export-eth-key $WALLET
```
Import Private Key kalian ke metamask, dan gunakan wallet yang di bikin untuk meminta faucet
- Open Web: https://testedge.haqq.network/

## Membuat Validator
Sebelum membuat validator, pastikan Anda memiliki setidaknya 1 ISLM = 1000000 aISLM,dan node Anda tersinkronisasi

Cek status node, apakah sudah false atau belum. Jika sudah false lanjut buat validator, kalo belum jangan lanjut ke step berikutnya
```
haqqd status 2>&1 | jq .SyncInfo
```

Lanjut buat validator

```
haqqd tx staking create-validator \
  --amount 10000000aISLM \
  --pubkey $(haqqd tendermint show-validator) \
  --moniker $NODENAME \
  --chain-id $HAQQ_CHAIN_ID \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1000000" \
  --gas-prices="0.025aISLM" \
  --from $WALLET \
  --node https://rpc.tm.testedge.haqq.network:443
  --keyring-backend file
```

## Edit Validator
```
haqqd tx staking edit-validator \
 --chain-id $HAQQ_CHAIN_ID \
 --details="deskripsibebas" \
 --website="isibebasbang" \
 --from $WALLET \
 --fees 0.025aISLM \
 --keyring-backend file
```

## Hapus Node (digunakan saat testnetnya udah ended)
```console
sudo systemctl stop haqqd
sudo systemctl disable haqqd
sudo rm /etc/systemd/system/haqq* -rf
sudo rm $(which haqqd) -rf
sudo rm $HOME/.haqqd* -rf
sudo rm $HOME/haqq -rf
sed -i '/HAQQ_/d' ~/.bash_profile
```
