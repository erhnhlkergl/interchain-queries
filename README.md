# STRIDE 9. GÖREV KODLARI 
** Uzun süreli bekleyişimiz sonunda son görevi aşağıdaki kodları kullanarak yapıyoruz ve artık STRIDE Testnetinin biteceğini günü bekliyoruz =) **

## İLK OLARAK BINARY DOSYALARINI EKLİYORUZ

```
cd $HOME
git clone https://github.com/Stride-Labs/interchain-queries.git
cd interchain-queries
go build
cd
sudo mv interchain-queries /usr/local/bin/icq
```

## DAHA ÖNCE ALDIĞIMIZ STRIDE RPC ve GRPC ile GAIA RPC ve GRPC KODLARINI AŞAĞIDAKİ KOD ÜZERİNDE DÜZENLİYORUZ. BURADA ÖNEMLİ NOKTA WALLET İSMİNİ DE DEĞİŞTİRMEKTİR.**

```
cd $HOME && mkdir .icq
sudo tee $HOME/.icq/config.yaml > /dev/null <<EOF
default_chain: stride-testnet
chains:
stride-testnet:
    key: WALLETNAME
    chain-id: STRIDE-TESTNET-4
    rpc-addr: STIDE RPC
    grpc-addr: STRIDE GRPC
    account-prefix: stride
    keyring-backend: test
    gas-adjustment: 1.2
    gas-prices: 0.001ustrd
    key-directory: /root/.icq/keys
    debug: false
    timeout: 20s
    block-timeout: ""
    output-format: json
    sign-mode: direct
gaia-testnet:
    key: WALLETNAME
    chain-id: GAIA
    rpc-addr: GAIA RPC
    grpc-addr: GAIA GRPC
    account-prefix: cosmos
    keyring-backend: test
    gas-adjustment: 1.2
    gas-prices: 0.001uatom
    key-directory: /root/.icq/keys
    debug: false
    timeout: 20s
    block-timeout: ""
    output-format: json
    sign-mode: direct
cl: {}
EOF
```

:BOOM: YUKARIDA KEY KISMINA KENDİ CÜZDANLARINIZ RPC VE GRPC KISMINA KENDİ KODLARINIZI GİRMEYİ UNUTMAYIN :BOOM:

**ICQ SERVİCE KODUNU TEK SEFERDE GİRELİM 

```
sudo tee /etc/systemd/system/icqd.service > /dev/null <<EOF
[Unit]
Description=Interchain Query Service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which icq) run --debug
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

**SERVİS KODLARINI GİREREK SERVİSİ BAŞLATALIM

```
sudo systemctl daemon-reload
sudo systemctl enable icqd
sudo systemctl restart icqd
```

**:BOOM: SON OLARAK TRANSFER KOMUTLATINI YAZARAK TXHASHLERİ ALIP EXPLORER'DA KONTROL EDİYORUZ**

```
rly transact transfer stride gaia 1000ustrd COSMOSADRESİNİZ channel-0 --path stride-gaia
```
```
rly transact transfer gaia stride 1000uatom strideadresi channel-0 --path stride-gaia
```

:PUNCH: HAYDİ GEÇMİŞ OLSUN :FIRE:

[EXPLORER POOLPARTY STRIDE](https://poolparty.stride.zone/STRIDE/) [EXPLORER POOLPARTY GAIA](https://poolparty.stride.zone/GAIA/) [EXPLORER NODESGURU](https://stride.explorers.guru//) 

