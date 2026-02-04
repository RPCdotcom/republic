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

# 5. Yetkiyi Verelim
```bash
chmod +x republicd_v2

# 6. Eski Binary'i Yedekliyip Yeniyi Koyalım
```bash
mv republicd republicd_v0.1.0_backup
mv republicd_v2 republicd
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
