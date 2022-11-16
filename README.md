# Tutorial testnet Q-Blockchain Incentivized

<p style="font-size:14px" align="right">
<a href="https://t.me/bangpateng_airdrop" target="_blank">Join Telegram Bang Pateng<img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
</p>

<p align="center">
  <img height="auto" width="auto" src="https://user-images.githubusercontent.com/38981255/202146176-5d0b26be-0a8f-411f-9f4d-0d87d89b4434.jpg">
</p>

## Referensi

[Dokumen resmi](https://docs.qtestnet.org/how-to-setup-validator/)

[Faucet](https://faucet.qtestnet.org/)

[Explorer](https://explorer.qtestnet.org/)

[Status validator](https://stats.qtestnet.org/)

[Server discord](https://discord.gg/GHPkcEAB)

## Spesifikasi

### Persyaratan perangkat keras

| Komponen | Spesifikasi minimal |
|----------|---------------------|
|CPU|Intel Core i3 or i5|
|RAM|4 GB DDR4 RAM|
|Penyimpanan|500 GB HDD|
|Koneksi|100 Mbit/s port|

| Komponen | Spesifikasi rekomendasi |
|----------|---------------------|
|CPU|Intel Core i7-8700 Hexa-Core|
|RAM|64 GB DDR4 RAM|
|Penyimpanan|2 x 1 TB NVMe SSD|
|Koneksi|1 Gbit/s port|

### Persyaratan perangkat lunak

| Komponen | Spesifikasi minimal |
|----------|---------------------|
|Sistem Operasi|Ubuntu 16.04|

| Komponen | Spesifikasi rekomendasi |
|----------|---------------------|
|Sistem Operasi|Ubuntu 18.04 atau lebih tinggi|

## Pasang dan Jalankan validator

### Pasang dependensi

```
apt install git docker docker-compose
```

### Unduh paket Q-Blockchain

```
git clone https://gitlab.com/q-dev/testnet-public-tools.git
```

### Masuk ke folder `testnet-validator`

```
cd testnet-public-tools/testnet-validator
```

### Buat kata sandi dompet

Kata sandi harus disimpan di folder `keystore`, jadi anda perlu membuat foldernya terlebih dahulu dengan perintah

```
mkdir keystore
```

Lalu buat file `pwd.txt` dan isi dengan kata sandi anda (kata sandi bebas)

```
nano keystore/pwd.txt
```

Setelah memasukan kata sandi, tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

Kemudian cek apakah kata sandi telah tersimpan

```
cat keystore/pwd.txt
```

> Jika kata sandi yang anda buat tadi tampil ke layar artinya kata sandi telah berhasil dibuat

### Buat dompet

```
docker run --entrypoint="" --rm -v $PWD:/data -it qblockchain/q-client:testnet geth account new --datadir=/data --password=/data/keystore/pwd.txt
```

Jika berhasil maka pesan seperti ini akan tampil di layar

```
Your new key was generated

Public address of the key:   0xb3FF24F818b0ff6Cc50de951bcB8f86b52287dac
Path of the secret key file: /data/keystore/UTC--2021-01-18T11-36-28.705754426Z--b3ff24f818b0ff6cc50de951bcb8f86b52287dac

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

Lalu simpan public keynya

### Klaim faucet

Pergi ke [sini](https://faucet.qtestnet.org/) lalu masukan alamat dompet yang tadi anda buat dan klaim Q Token

> jika terjadi error maka ulangi terus sampai dapat, anda juga bisa klaim token yang lain

### Konfigurasi `.env`

Jalankan perintah ini

```
nano .env
```

Lalu ganti variabel dibawah

| Variabel | Keterangan |
|----------|------------|
|ADDRESS|Ganti dengan alamat dompet tadi lalu hilangkan 0x)|
|IP|Ganti dengan alamat IP VPS anda (yang digunakan untuk login VPS)|

Setelah itu tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

### Konfigurasi `config.json`

Jalankan perintah ini

```
nano config.json
```

Lalu ganti address dan password dengan alamat dompet dan kata sandi anda

Setelah itu tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

### Taruh stake di validator contract

```
docker run --rm -v $PWD:/data -v $PWD/config.json:/build/config.json qblockchain/js-interface:testnet validators.js
```

### Daftarkan validator

Jalankan perintah ini

```
nano docker-compose.yaml
```

lalu dibagian `entrypoint` setelah `geth` tambahkan ini

```
"--ethstats=NAMA_VALIDATOR:TESTNET_ACCESS_KEY@stats.qtestnet.org",
```

Untuk mendapatkan `TESTNET_ACCESS_KEY` anda bisa tanyakan ke grup discordnya

Setelah itu tekan <kbd>CTRL</kbd> + <kbd>x</kbd> + <kbd>Y</kbd> lalu tekan <kbd>Enter</kbd>

### Jalankan validator

Jalankan perintah dibawah untuk menjalankan validator

```
docker-compose up -d
```

Lalu cek log

```
docker-compose logs -f --tail "100"
```

Cari nama validator anda [disini](https://stats.qtestnet.org/)

### Done

Source : https://github.com/bayy420-999/
