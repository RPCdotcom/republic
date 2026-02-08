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
```
```bash
mkdir -p $HOME/.republicd/cosmovisor/genesis/bin
```
```bash
curl -L "https://media.githubusercontent.com/media/RepublicAI/networks/main/testnet/releases/${VERSION}/republicd-linux-amd64" -o republicd
```
```bash
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
PEERS="8567f9acbb313978a16b1626fe0e997bbcd97990@162.243.109.138:26656,a02d1c8e9f481f30127ce0ef89c9e490f61a4e2e@38.49.214.70:26656,60ed36b4bc3c047135f101c7829e2f7a9905ff8e@154.12.116.167:13356,27fe28b729c5f8b2728ac11322dfa43cfa193af9@154.12.117.7:13356,7e483c0ab1cbf60a1056263903dc3a3269244141@38.49.214.94:26656,38fa0132bd791dddf5a4db7c440af494af9ee3b2@34.61.170.254:26656,67ecda5dfaf5aa5519afdac580c832f0118a730f@62.171.142.162:26656,90cabe6f1bd8bd4eafec781f224cfac725ae5391@152.53.230.81:47656,cdae43b6f4ee3d3824648ae2bcb8ee21e689396e@89.167.25.39:43656,ab6065d07a85c9c6f10d8084a27baaecbb6cd529@65.108.231.223:43656,d7fc881ac77368920ecef7da3ba700c2146a7374@46.225.87.25:43656,398279493cf9c41108b867cddfcd786b275e153c@185.100.54.13:26656,1fc361b76cb5d3190027e18299a22e3dcb689dd9@172.31.30.32:26656,5d28825836938c8ad6012eb9f0f2215421a77e31@173.212.224.154:26656,f751d819c6c50a9a4232e89e76d69a111ded570e@89.167.44.231:26656,6313f892ee50ca0b2d6cc6411ac5207dbf2d164b@95.216.102.220:13356,af276746c9d05a31160fa09dac4f03ca7cbb5ab1@152.53.161.155:13356,02c54e90a23d7e7a7f2eb1197f3b57602e0c1244@198.7.125.48:26656,4e14a1edc972ed3f4c03eae8434cb3997b342029@46.224.213.11:43656,938b2dffbd2fc169806f9ab3d01b473281c7fc82@157.180.52.245:48656,561fd4716968ee71992adeca4c29cee338327009@89.34.219.104:26656,cbae02c6cc882a222ccd7939d4d93c4b3f7077a9@65.108.14.235:36656,a6c1cc71dae002bce81a42b2d2a154fbe1046c54@89.167.13.7:43656,4b02d8dc14065e21cefd7a93e7efeee09508e292@109.123.252.55:26656,3cc74ebe26e070dc898b1e074161ffafa827081b@152.53.230.108:17656,d3b15134e289878fb6b8e8f54b69a8be9060a418@37.27.109.41:26656,282a75919f493c98f705a25e17165c80c177598a@217.216.111.83:26656,543c7e614249cc7038850adb3f9af75b31690179@152.53.53.5:43656,bb8dd41fc4731fd1b99bb054103c5c9526433bdc@149.5.246.217:43656,09f1d1e1c69200f3c77e7266323775af56f9172c@77.42.89.219:43656,f41590c5fd116dc2e52fafb341e9a9b415566534@87.255.8.15:26656"

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
PUBKEY=$(jq -r '.pub_key.value' $HOME/.republicd/config/priv_validator_key.json)

cat > validator.json << EOF
{
  "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"$PUBKEY"},
  "amount": "20000000000000000000arai",
  "moniker": "validator ismin",
  "identity": "keybaseidlazimyoksabosbirak",
  "website": "web sitesi yoksa twitter link koy",
  "security": "mailadresi",
  "details": "validatoraciklaması",
  "commission-rate": "0.05",
  "commission-max-rate": "0.15",
  "commission-max-change-rate": "0.02",
  "min-self-delegation": "1"
}
EOF

republicd tx staking create-validator validator.json \
--from $REPUBLIC_WALLET \
--chain-id raitestnet_77701-1 \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices "1000000000arai" \
--node tcp://localhost:43657 \
-y

```

- Validatör Explorer Kontrol ; 

- Link : https://explorer.republicai.io/validators


## Yedeklenecekler - PC'nize USB'nize vb. ;

- Cüzdan kelimeleri.

- Config İçinde ; 

<img width="948" height="585" alt="image" src="https://github.com/user-attachments/assets/f66b32ea-806c-492f-98ba-e142ec9752cf" />

- data içinde ; 

<img width="918" height="444" alt="image" src="https://github.com/user-attachments/assets/412665f6-5b45-4e2a-bd67-8500d1dcf47d" />



## Güncel Peer & İçin ; 

```bash
URL="https://rpc.republicai.io/net_info"
```

```bash
response=$(curl -s $URL)
```

```bash
PEERS=$(echo $response | jq -r '.result.peers[] | select(.remote_ip | test("^[0-9]{1,3}(\\.[0-9]{1,3}){3}$")) | "\(.node_info.id)@\(.remote_ip):" + (.node_info.listen_addr | capture(":(?<port>[0-9]+)$").port)' | paste -sd "," -)
```

```bash
echo "PEERS=\"$PEERS\""
```

```bash
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.republicd/config/config.toml
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable republicd
sudo systemctl start republicd
```

## Çeşitli Kodlar & Delege vb.

#### Delege ; 

- Token Karşılığı ; 10000000000000000000arai : 10 Adet Token ( Komutta 10 Adet Olan Ekli ) 
- Token Karşılığı ; 1000000000000000000arai : 1 Adet Token

```bash
republicd tx staking delegate validatorvaloperadresi \
10000000000000000000arai \
--from wallet \
--chain-id raitestnet_77701-1 \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 1000000000arai \
--node tcp://localhost:43657 \
-y
```

- Valoper Adres Nasıl Alınır ?

- Explorer : https://explorer.rpcdot.com/republic-testnet/staking

<img width="1047" height="861" alt="image" src="https://github.com/user-attachments/assets/d885087b-204f-405b-93c0-b04ab4d20a5b" />

## Unjail ; 

```bash
republicd tx slashing unjail \
--from wallet \
--chain-id raitestnet_77701-1 \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 1000000000arai \
--node tcp://localhost:43657 \
-y
```
