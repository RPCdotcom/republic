
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
wget -O republicd_v2 https://github.com/RepublicAI/networks/releases/download/v0.2.0/republicd-linux-amd64
```

# 4. Doğrulayalım
```bash
echo "0788832a9f88fedfe1e2896391dd4bdb944aca85ae0632f3d30e09743a455b65  republicd_v2" | sha256sum -c
```
# 5. Yetkiyi Verelim
```bash
chmod +x republicd_v2
```
# 6. Eski Binary'i Yedekliyip Yeniyi Koyalım
```bash
sudo mv republicd republicd_v0.1.0_backup
sudo mv republicd_v2 republicd
```

- CLI Komutları için usr local bin'ide Güncelleyelim 

```bash
cd
```
```bash
cd /usr/local/bin
mv republicd republicd_backup_pre_v0.1.0
```
```bash
cp $HOME/.republicd/cosmovisor/genesis/bin/republicd /usr/local/bin/republicd
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
# 2. Yeni binary’yi indir (v0.2.0)
```bash
cd /usr/local/bin

wget -O republicd_v0.2.0 https://github.com/RepublicAI/networks/releases/download/v0.2.0/republicd-linux-amd64
```

# 3. SHA256 checksum doğrula ✅
```bash
echo "0788832a9f88fedfe1e2896391dd4bdb944aca85ae0632f3d30e09743a455b65  republicd_v0.2.0" | sha256sum -c
```

# 4. Yetkiyi Verelim
```bash
chmod +x republicd_v0.2.0
```
# 5. Eski Binary'i Yedekliyip Yeniyi Koyalım
```bash
sudo mv republicd republicd_v0.1.0_backup
sudo mv republicd_v0.2.0 republicd
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
