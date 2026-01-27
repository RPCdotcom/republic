<img width="1500" height="500" alt="image" src="https://github.com/user-attachments/assets/bca860a5-7a59-4cf5-85b3-701124d08a96" />

# Republic Validator Node Kurulum

| X        | Minimum              | Önerilen |
|------------------|----------------------------|----------------------------|
| **CPU**          | 4+ | 8++ |
| **RAM**          | 16+  | 16++ GB                   |
| **Disk**      | 500GB NVME SSD| 500+ NVME GB SDD                   |
| **UBUNTU**      | UBUNTU 24.04 | + |


- Web : https://republicai.io
- X : https://x.com/republicfdn

- Web : https://rpcdot.com
- X : https://x.com/rpcdot

## 1. Server Güncelleme : 

```bash
sudo apt update -y && sudo apt upgrade -y
```
## 2. Topluca Paketleri İndirme :

```bash
sudo apt install htop ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev tmux iptables curl nvme-cli git wget make jq libleveldb-dev build-essential pkg-config ncdu tar clang bsdmainutils lsb-release libssl-dev libreadline-dev libffi-dev jq gcc screen file nano btop unzip lz4 -y
```

## 3. Go & Cosmovisor ( Yoksa )
```bash
cd $HOME
wget https://go.dev/dl/go1.22.3.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.22.3.linux-amd64.tar.gz
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

## 4. Mini Küçük Ayarlar Port - Wallet İsmi ben 43 Yaptım sunucuda 43 doluysa değiştirebilirsiniz.
```bash
echo "export REPUBLIC_WALLET='wallet'" >> $HOME/.bash_profile
echo "export REPUBLIC_PORT='43'" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## 5. Binary Çekelim
```bash
VERSION="v0.1.0"

mkdir -p $HOME/.republicd/cosmovisor/genesis/bin

curl -L "https://media.githubusercontent.com/media/RepublicAI/networks/main/testnet/releases/${VERSION}/republicd-linux-amd64" -o republicd
chmod +x republicd
```

```bash
mv republicd $HOME/.republicd/cosmovisor/genesis/bin/
ln -s $HOME/.republicd/cosmovisor/genesis $HOME/.republicd/cosmovisor/current
```
```bash
sudo ln -s $HOME/.republicd/cosmovisor/genesis/bin/republicd /usr/local/bin/republicd
```

- Kontrol Çekelim ; 
```bash
republicd version
```

## 6. Init ; 

- Burada YOUR_NODE_NAME kısmını kendi validatör adınız ile değiştirin. İngilizce karakter kullanımını unutmayın. 

```bash
republicd init YOUR_NODE_NAME --chain-id raitestnet_77701-1 --home $HOME/.republicd
```

## 7. Genesis : 

```bash
curl -s https://raw.githubusercontent.com/RepublicAI/networks/main/testnet/genesis.json > $HOME/.republicd/config/genesis.json
```

## 8. Config & Port ; 

- Config : 

```bash
sed -i.bak -e "s%:26658%:${REPUBLIC_PORT}658%g;
s%:26657%:${REPUBLIC_PORT}657%g;
s%:6060%:${REPUBLIC_PORT}060%g;
s%:26656%:${REPUBLIC_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${REPUBLIC_PORT}656\"%;
s%:26660%:${REPUBLIC_PORT}660%g" $HOME/.republicd/config/config.toml
```

- App

```bash
sed -i.bak -e "s%:1317%:${REPUBLIC_PORT}317%g;
s%:8080%:${REPUBLIC_PORT}080%g;
s%:9090%:${REPUBLIC_PORT}090%g;
s%:9091%:${REPUBLIC_PORT}091%g;
s%:8545%:${REPUBLIC_PORT}545%g;
s%:8546%:${REPUBLIC_PORT}546%g;
s%:6065%:${REPUBLIC_PORT}065%g" $HOME/.republicd/config/app.toml
```

## 9. Peers & Seeds 

```bash
SEEDS=""
PEERS="e281dc6e4ebf5e32fb7e6c4a111c06f02a1d4d62@3.92.139.74:26656,cfb2cb90a241f7e1c076a43954f0ee6d42794d04@54.173.6.183:26656,dc254b98cebd6383ed8cf2e766557e3d240100a9@54.227.57.160:26656"

sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.republicd/config/config.toml
```

## 10. Pruning - Disk dostudur dostlar daha az yer kaplamasını sağlar

```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.republicd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.republicd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.republicd/config/app.toml
```

## 11. Indexer - Gidipte büyük RPC sağlayıcısı değilseniz gerek yok daha hafifletir. Açarsak ne olur ? Birisi rpc'niz üzerinden tx bakmak istediğinde detaylar gözükür - kapalıysa tx detay göremez ama rpc sorunsuz devam eder : 

```bash
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.republicd/config/config.toml
```

## 12. Gas

```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "250000000arai"|g' $HOME/.republicd/config/app.toml
```

## 13. Cosmovisro System

```bash
sudo tee /etc/systemd/system/republicd.service > /dev/null <<EOF
[Unit]
Description=Republic AI Node
After=network-online.target

[Service]
User=$USER
Environment="DAEMON_NAME=republicd"
Environment="DAEMON_HOME=$HOME/.republicd"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=true"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
ExecStart=$HOME/go/bin/cosmovisor run start --home $HOME/.republicd --chain-id raitestnet_77701-1
Restart=always
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

## 13. Başlat ; 
```bash
sudo systemctl daemon-reload
sudo systemctl enable republicd
sudo systemctl start republicd
```

## 14. Loglar : 
```bash
journalctl -u republicd -f
```

- Loglardaki block sayısı explorer ile uyuşuyorsa SYNC olmuşsunuz demektir.
- Explorer : https://explorer.republicai.io/blocks

+ Kontrol ; 
```bash
republicd status --node tcp://localhost:43657 | jq '.sync_info'
```

- ÖRnek Sonuç ; 

<img width="630" height="199" alt="image" src="https://github.com/user-attachments/assets/28199746-3346-4976-b6d6-696b345a2eec" />


## Genel Cüzdan Oluşturma & Validatör Kurulum ; 

```bash
republicd keys add $REPUBLIC_WALLET
```

- Sizden cüzdan için şifre isteyecek yazacaksınız unutmayacağınız birşey olsun, sonrasında bir daha doğrulamak için isteyecek yazacaksınız. Görünmezdir o yüzden yazdığınızdan emin olun.
- Karaladığın yerde cüzdan kelimeleriniz var onları kimseyle paylaşmayın kaydedin.

<img width="931" height="250" alt="image" src="https://github.com/user-attachments/assets/1ab87988-a630-4c3d-ac1e-0f0bd27b2d74" />


- Cüzdana faucet lazım DC Faucet kanalından adminden token istiyoruz ; 

<img width="1080" height="382" alt="image" src="https://github.com/user-attachments/assets/bfb8669d-edb6-408a-be99-cd1e7ac49f42" />

- Validatör Oluşturma ;

```bash
republicd tx staking create-validator \
--amount 1000000000000000000000arai \
--pubkey="$(republicd tendermint show-validator --node tcp://localhost:43657)" \
--moniker "Validator Adiniz" \
--identity "keybaseidnizyoksabosbirakabilirsiniz" \
--details "Aciklamakismi" \
--website "website linkiniz yoksa twitter linkiniz" \
--commission-rate "0.05" \
--commission-max-rate "0.15" \
--commission-max-change-rate "0.02" \
--min-self-delegation "1" \
--from $REPUBLIC_WALLET \
--chain-id raitestnet_77701-1 \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices "250000000arai" \
--node tcp://localhost:43657
```

- Validatör Explorer Kontrol ; 

- Link : https://explorer.republicai.io/validators


## Yedeklenecekler - PC'nize USB'nize vb. ;

- Cüzdan kelimeleri.

- Config İçinde ; 

<img width="948" height="585" alt="image" src="https://github.com/user-attachments/assets/f66b32ea-806c-492f-98ba-e142ec9752cf" />

- data içinde ; 

<img width="918" height="444" alt="image" src="https://github.com/user-attachments/assets/412665f6-5b45-4e2a-bd67-8500d1dcf47d" />

