# Prepare intensivized testnet Haqq

**Update packages and install required packages**
```bash
sudo apt update && sudo apt upgrade -y && sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```

**Install binary project**
```bash
cd $HOME && git clone https://github.com/haqq-network/haqq && cd haqq && make install
```

**Init moniker and set chainid** (ganti namavalidator bebas bang)
```bash
haqqd init namavalidator --chain-id haqq_54211-2 && haqqd config chain-id haqq_54211-2
```

**Create wallet** (jangan lupa simpan address, phrase atau menemonic kalian)
```bash
haqqd keys add wallet
```

**Add genesis account**
```bash
haqqd add-genesis-account wallet 10000000000000000000aISLM
```

**Create gentx** (namavalidator ganti pake yang tadi dibuat nama validatornya)
```bash
haqqd gentx wallet 10000000000000000000aISLM \
--chain-id=haqq_54211-2 \
--moniker="namavalidator" \
--commission-max-change-rate 0.05 \
--commission-max-rate 0.20 \
--commission-rate 0.05 \
--website="webnya bebas" \
--security-contact="email lu" \
--identity="bebas" \
--details="dekskripsi bebas"
```

Jika sudah langsung isi form : https://haqq-network.typeform.com/to/zEgmX3TO gunakan address yang udah dibuat tadi
