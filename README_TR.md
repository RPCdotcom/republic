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
VERSION="v0.2.1"

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
PEERS="cfb2cb90a241f7e1c076a43954f0ee6d42794d04@172.31.26.240:26656,1b0eac045d525cf18ce9df5e58b59f422e1ac147@172.31.22.66:26656,dc254b98cebd6383ed8cf2e766557e3d240100a9@172.31.28.158:26656,5909d46879ef1a352cfee63a93552432dd4bbeb7@172.31.21.191:26656,cc13360aec4a5ef532fce577f70bbc8fd665c211@65.108.110.246:26656,20a9721219f5056eacf89c19fd60c1999b398ea3@65.21.88.99:18656,08de17e0a650192861a6dd2ef63bbaca73f73592@34.55.100.110:26656,a5b245a4734167c9377ce3f7828947240cc7fd60@154.12.117.35:23656,a5d2fe7d932c3b6f7c9633164f102315d1f575c6@195.201.160.23:13356,bd892704441b02388b0527a0d15e40fee89b05b9@159.195.26.108:18656,f4435ef54602a368802f662a5a6a2b7b923f7071@103.171.146.57:26656,1999732342c6c89de4112f82008766bf3342aba2@104.152.210.127:55856,525ad7484153172c199433ed8ac7af26c2b228bc@14.169.16.64:26656,a698525f90433d6069a2fcc22b2b3f1cc13593cc@192.99.54.87:26656,194d200f41374b508bd79c062b29c75937c37103@38.49.209.147:26656,9ab906725d5a7f6eb513b4f0cbbe7ca357ebceea@213.109.161.44:26656,6ab1b623e0c1a0f2d3099cc502d9f3b4f9bc8a73@95.216.102.220:13356,d97efec4de6bcfa2c81052c0e7ebeff2a37655ea@34.51.40.253:26656,2c9641378e9dacc49b269b0efb807b475d033e56@89.167.15.180:26656,6dd3a10b5157d2ac68bd76922de849244ea1f4fa@65.109.55.116:34656,ec9cbd29992dea64e8adb26af1ebc5cc6a958875@34.44.164.13:26656,d1a6a7cfbd80805d486e12560ba3af6d868f108a@72.61.158.34:26656,65cf55232924bebfdd02c32446df5a101188a05e@161.97.95.86:26656,e2e2e02d42860e421294485740b48baf9e3e0b0b@179.43.150.242:26656,31e30d2dd6d10933c5a7281f7dffeb09d93fdf12@154.12.116.93:26656,f6d854d1b5d3cc3cc73fb46fbfe53e0df1c05d02@154.12.118.215:26656,cc2504dd639c01038444b309f4549139a43b77d3@206.168.81.49:26656,f990e33b674c0e91dc74e1067785bfe493985a34@182.9.2.84:26656,233fde2cfd5d8f87c15a2bf6ac0b19697e53897e@107.222.215.224:26656,b9f252664aad3370c4e186409c1fda71505a43b7@152.53.251.43:13356,d42950d61ff958d9f3a12549ef7a535b10357ce4@38.49.212.156:26656,c5f9653155d9095901c8044dc01fadf49212f350@45.143.198.6:26656,93d1e37cf97435491aaca98e04e18d2f6df99192@103.138.70.189:26656,d5401bc2fe46760e06f5c3af62b57463d83f90ad@89.167.26.162:26656,d5f910e658965da487e5537eac6a382f807e6a03@154.12.118.123:26756,9a8f0730cf929ea6655ca970c0a8ba81c19fb753@154.12.116.115:26656,5b4af65c46e97cf8ee991f07dca1f02d55cb2256@82.112.237.186:26656,6936fb0987f24160fcf0d6df2715dc578aae4c09@157.90.1.91:43656,c17a8d3bd27bce5ad90c3215e52671461697c19e@38.49.212.121:26656,d8798195b453a24b7d31b8075ac46de93267e6ea@2.50.222.30:26656,462fd3cf9880438ac732f221bc271b22a0ce7132@172.104.155.28:26656,2a939e0322783f1b85a16cf768091c79430457f1@138.197.115.112:26656,e5d2fd28460427dcb15efcd3b27843593cbedcc1@114.29.239.131:26656,b97f371120741a76abc21baea936659f9204f8b1@146.19.24.206:43656,7971d6669264775653766efcbcb87f8e9664faa6@192.151.150.34:13356,ea12408721fd0ed1865021add1f4789988783643@2.56.97.139:13356,007ee0fafbad23ec7e132a708281bdbdb7d89350@69.62.113.85:26656,033c76d723335e2a157236f75aefe98a83973f77@38.49.213.142:43656,0729cae89fbdb4e2065423a768f3a4e1b42ab3a7@38.49.214.35:43656,4e14a1edc972ed3f4c03eae8434cb3997b342029@46.224.213.11:43656"

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
