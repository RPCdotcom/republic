
# COSMOVİSOR İLE KURANLAR İÇİN ; 


# 1. Nodeyi Durduralım
```bash
sudo systemctl stop republicd
```
# 2. Cosmovisor Genesis'e Gidelim
```bash
cd $HOME/.republicd/cosmovisor/genesis/bin
```

# 3. Yeni Binary İndirelim
```bash
wget -O republicd_v030 https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64
```

# 4. Doğrulayalım
```bash
echo "bf0c88fda3ec40d8b991f87105c46ac6ddd7901d735213748de2c14e1b63a2a5 republicd_v021" | sha256sum -c
```

<img width="1244" height="88" alt="image" src="https://github.com/user-attachments/assets/f1fc330b-c713-4299-99a1-29c92d52ca0a" />

# 5. Yetkiyi Verelim
```bash
chmod +x republicd_v030
```
# 6. Eski Binary'i Yedekliyip Yeniyi Koyalım
```bash
sudo mv republicd republicd_v0.2.1_backup
sudo mv republicd_v030 republicd
```

- CLI Komutları için Güncelleme

```bash
cd
```
```bash
cd /usr/local/bin
sudo mv republicd republicd_backup_pre_v0.2.1
```
```bash
sudo cp $HOME/.republicd/cosmovisor/genesis/bin/republicd /usr/local/bin/republicd
```
```bash
chmod +x /usr/local/bin/republicd
```

# 7. Başlatalım
```bash
sudo systemctl daemon-reload
sudo systemctl enable republicd
sudo systemctl start republicd
```

# 8. Logları Kontrol Edelim
```bash
sudo journalctl -u republicd -f
```

<img width="480" height="144" alt="image" src="https://github.com/user-attachments/assets/aa1011c8-7171-4c5e-a4e9-0ae3d6abda4c" />


# COSMOVİSORSÜZ KURANLAR İÇİN ; 


# 1. Nodeyi Durduralım
```bash
sudo systemctl stop republicd
```
# 2. Yeni binary’yi indir (v0.2.1)
```bash
cd /usr/local/bin

wget -O republicd_v0.3.0 https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64
```

# 3. SHA256 checksum doğrula ✅
```bash
echo "d10991b623fa62ee0f3c42ad15abbaa246f89d73a82736fad090e607b7cc4b8f  republicd_v0.3.0" | sha256sum -c
```
<img width="1244" height="88" alt="image" src="https://github.com/user-attachments/assets/a12dd09c-5bed-48b1-b35f-e505ef8ada10" />


# 4. Yetkiyi Verelim
```bash
chmod +x republicd_v0.3.0
```
# 5. Eski Binary'i Yedekliyip Yeniyi Koyalım
```bash
sudo mv republicd republicd_v0.2.1_backup
sudo mv republicd_v0.3.0 republicd
```
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
