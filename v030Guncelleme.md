
# COSMOVİSOR İLE KURANLAR İÇİN ; 

- Upgrade height: 326250 ( Block )

- Upgrade name: v0.3.0


# 1. Güncelleme İçin Gereklilikler
```bash
mkdir -p $HOME/.republicd/cosmovisor/upgrades/v0.3.0/bin
```
# 2. Cosmovisor Upgrades'e Gidelim
```bash
cd $HOME/.republicd/cosmovisor/upgrades/v0.3.0/bin
```

# 3. Yeni Binary İndirelim
```bash
wget -O republicd https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64
```

# 4. Doğrulayalım
```bash
echo "bf0c88fda3ec40d8b991f87105c46ac6ddd7901d735213748de2c14e1b63a2a5  republicd" | sha256sum -c
```

<img width="1244" height="88" alt="image" src="https://github.com/user-attachments/assets/f1fc330b-c713-4299-99a1-29c92d52ca0a" />

# 5. Yetkiyi Verelim
```bash
chmod +x republicd
```

# 6. Version Kontrol

```bash
./republicd version
```

- 326250. Blockta otomatik olarak güncellenecektir. Cosmovisor.

# Normal Kurulum için bu işlem 326250. Blocktan Sonra YAPILMALIDIR.

# 1. Nodeyi Durduralım
```bash
sudo systemctl stop republicd
```
# 2. Yeni binary’yi indir (v0.3.0)
```bash
cd /usr/local/bin

sudo mv republicd republicd_backup_v0.2.1

sudo wget -O republicd https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64

sudo chmod +x republicd
```

# 3. SHA256 checksum doğrula ✅
```bash
echo "bf0c88fda3ec40d8b991f87105c46ac6ddd7901d735213748de2c14e1b63a2a5  republicd" | sha256sum -c
```
<img width="1244" height="88" alt="image" src="https://github.com/user-attachments/assets/a12dd09c-5bed-48b1-b35f-e505ef8ada10" />

# 6. Başlatalım
```bash
sudo systemctl daemon-reload
sudo systemctl enable republicd
sudo systemctl start republicd
```

# 8. Logları Kontrol Edelim
```bash
sudo journalctl -u republicd -f
```
