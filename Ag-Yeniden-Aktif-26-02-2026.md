## 1. Peerleri Güncelleyelim ; 

```bash
peers="6313f892ee50ca0b2d6cc6411ac5207dbf2d164b@peers-t.republic.vinjan-inc.com:13356,1fc361b76cb5d3190027e18299a22e3dcb689dd9@54.159.96.158:26656,7fef6e3bb5c254c777449e09e9cf0ee40f4cdee3@195.201.160.23:13356"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.republic/config/config.toml
```

## 2. Güncel bir Snap Üzerinden Blocklara Yetişelim ; 

- Bu işlemi yapmadan önce bilgilerinizin yedekli olduğundan emin olunuz.

```bash
sudo systemctl stop republicd
cp $HOME/.republicd/data/priv_validator_state.json $HOME/.republicd/priv_validator_state.json.backup
republicd comet unsafe-reset-all --home $HOME/.republicd --keep-addr-book
curl -L https://snapshot.vinjan-inc.com/republic/latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.republicd
mv $HOME/.republicd/priv_validator_state.json.backup $HOME/.republicd/data/priv_validator_state.json
```
```bash
sudo systemctl restart republicd
sudo journalctl -u republicd -f -o cat
```

## 3. Blockları Yakalamışmıyız Kontrol Edelim ; 

```bash
republicd status --node tcp://localhost:43657 | jq '.sync_info'
```

- False yazacak ama loglardan da kontrol edin durum ne diye.
- Güncel Block Bakmak İçin : https://explorer.rpcdot.com/republic-testnet/staking

<img width="642" height="207" alt="image" src="https://github.com/user-attachments/assets/4829f8cb-4f0b-44e0-a535-4f4b3229ff14" />


## 4. Jail'den Çıkalım ; 

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

- Eğer Port'u 43 Ayalamayıp Direkt Kurduysanız ; 

```bash
republicd tx slashing unjail \
--from wallet \
--chain-id raitestnet_77701-1 \
--gas auto \
--gas-adjustment 1.5 \
--gas-prices 1000000000arai \
--node tcp://localhost:26657 \
-y
```
