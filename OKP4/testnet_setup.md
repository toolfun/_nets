# 25 okt 2022. TESTNET now for the 150 "druids". Guide no more relevant
## 🚧🚧 guide is under construction
____

## OKP4 Setup

### links
- **website**    
https://okp4.network    
- **github**    
https://github.com/okp4
- **explorers**    
https://okp4.explorers.guru    
https://explorer.bccnodes.com/okp4

____

### general
```sudo apt update && sudo apt upgrade -y
sudo apt install curl -y
source $HOME/.bash_profile
```

### install go
```
if ! [ -x "$(command -v go)" ]; then
  ver="1.19.2"
  cd $HOME
  wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
  rm "go$ver.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
fi
```
```
go version
```

### set variables, set yours
```
OKP4_NODENAME=oneliner
OKP4_WALLET=oneliner
OKP4_PORT=24
OKP4_CHAIN_ID=okp4-nemeton
```
```
echo "export OKP4_NODENAME=$OKP4_NODENAME" >> $HOME/.bash_profile
echo "export OKP4_WALLET=$OKP4_WALLET" >> $HOME/.bash_profile
echo "export OKP4_CHAIN_ID=$OKP4_CHAIN_ID" >> $HOME/.bash_profile
echo "export OKP4_PORT=${OKP4_PORT}" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### install okp4d
```
git clone https://github.com/okp4/okp4d.git
cd okp4d && git checkout v2.2.0
make build
mv ./target/dist/okp4d /usr/local/bin/
```

### init
```
okp4d init "$OKP4_NODENAME" --chain-id=OKP4_CHAIN_ID
```

### add seed and peers 
```peers="994c9398e55947b2f1f45f33fbdbffcbcad655db@okp4-testnet.nodejumper.io:29656,370a5d94910f2367ce15c7af07b4a4f552824085@138.68.158.147:26656,0de35a0f8beae7107271d8691a89fef55afd571f@194.163.133.140:26656,4de1cff160aac3c76c0fa5618ffdcdcffcf37cbe@207.244.236.52:26656,0007478807ab460738faac0758d94bdabeffd5d9@167.235.143.135:26656,6401459caad72c00fcb6704149dbe7025213281c@54.82.98.80:26656,9aad7337869f0f3caf5744d0679859670316f381@65.108.63.58:26656,2bfd405e8f0f176428e2127f98b5ec53164ae1f0@142.132.149.118:26656,5a71f2ed3a7a69a15f7dfcb3a92ab4d7f9e3db76@95.217.235.147:26656,5c5b4e55c3af67875efba1b78fbeee77db54cef5@88.198.39.43:26776,bbf1fb5c94a9938a09af845b8cc89cac69257e33@185.202.238.254:26656,38db262effc2a904a0a902edfb99c55f6825edd8@52.231.156.226:26656,9917f412470344841e913415a5ea5da9da96a8fa@65.108.238.217:11014,92a51dc2424d5d19412fa1c1fcafb8af3b5c4137@77.51.200.79:26656,75d27d10f38155f2ffbcd89b7323badf4f3c7baf@65.108.253.94:26656,4b2e5c9baa158478f3e76dc2af35082b8cc4eb25@46.101.9.115:26656,394ee378f82a2c7e73dbb601b4e266d3f5185b47@65.108.124.54:37656,33cdd436b0dfaefb9fe9f834330ccbc0510120c3@5.161.54.117:26656,5fc8c51583a51477114d7df0e553b02096e2c860@161.35.168.89:26656,a94957731607fac7639c6b767aa449e60d0941a9@4.235.120.91:46656,2cb6f390acddc70609f52c285911d07a1a6bbccd@117.1.132.79:26656,c8b6a4a0d7649a30dc96eeebc3f99a3c6f922ea0@38.242.219.104:26656,0c475a2dafa4e2fff52114ce5d84b1144c4296c6@138.68.128.159:26656,cdd1ca4df40df0dbba1c095b89d51090e811b29d@5.161.52.56:26656,dd4cdfde3788c9f6387ea7f32ecacb0a8a4932e7@65.21.247.135:26656,2bdac5e68872b28fed6a3ae114612bfe643c25c9@38.242.146.53:26656,f5263e4ab184f6990aacfaa5388a4cb6c7f6dd1c@45.85.249.116:26656,b800cd6918d9cb7c769bf42fb257ccaef7b571b0@65.108.230.161:11156,9e3109ba10d8cdb18d37dde787665ad1b38a85ed@65.108.235.107:12656,81cf0d0ae52dc92f1a0f89c306a37ae2b57cffe8@20.243.106.99:26656,8a6f55d50bb1b22483ae40ddd19d40486f720c42@5.161.93.248:26656,e488d1126edce82f9faa68f201811df6d2006d8e@38.242.244.72:26656,f27cfc89e60166c4dcb859710a5d12051fb20fbd@154.12.225.88:26656,c3e1646029109c374bedb4c1737c86a8d389a419@146.190.209.11:26656,2590f28592a97137de0b6f68043225e2890054b0@65.108.229.225:37656,89a8c6e1099fd6c4abd6522c574b939b2045f7fb@66.175.234.3:26656,0b7a7bb8251ab238bf292911055cd752138546de@194.5.152.172:26656,5dd460566b63d929fc8dd1e1ae52c9a26920fab3@162.55.43.133:26656,7da790c663d678cb064ff4fba04556dcf18bda2c@65.109.70.23:17656,7447f19178cbb41330bf7112a1b2e17ae6007071@199.204.45.15:2456,270a714b6ea789d9a6472f158118043e643a5491@77.232.37.67:26656,2911ca079d686d75fea411e5dea83fb305269f93@116.202.161.165:27656,ee16105fc680d8690c395cee8f01456464a09b11@5.161.72.10:26656,aa5cc52860381e60f38e88fc3c7f47e04078eaf4@45.87.104.113:61656,8db12a66e6381fad19b3d8d96cc9371ba7e2ff25@38.242.247.183:26656,16a604f8433df064bfa9c958c20dde16d9f2f0ec@165.227.236.144:26656,cc15ceec925e511f9f660deb3671770341abee18@86.48.20.122:26656,080f51ee75de47fa74aa35b87cc46051da47b20b@165.22.203.183:26656,873851ddeee527352fc16802c866e41cd36565d8@65.109.30.197:22656,b8fddd530b2d8347212615b6a68c447aba0aed64@161.35.37.194:26656,de245278be4c3540f0a6a867c4bac83155b4ebac@178.62.30.239:46656,f1fc62c9554a2682cba2961bde18c19ebea5c1a0@34.125.207.91:26656,9b8cd8bac2fa12f68e2c759042c982901f9527fe@86.48.1.142:26656,b3698769d3fcede4d6b383aab606d09ae890d679@206.189.120.109:26656,6fc2bb503dc47b5a9ce2b51582f56259f180b09b@174.138.34.33:26656,0f68684eeb29b62cc650ae48ad944e573c6a6bec@38.242.238.46:26656,02d428f4933c832e4a24307704b5181bc7cd43f0@137.184.225.125:2456,49554e7e24afbdf04b5ba50ec0266f0aef989280@40.114.199.194:26656,c486053dc7c96e831781ca4c8b3c8c3da04b9059@84.54.23.199:26656,3664b233b8d63ef9f65733271fc2a646716a4f26@190.2.155.67:28656,7dc9e97aba15de7055d4ea98114aa231bed4f064@34.127.75.186:26656,c0864edb1e36c52dbee47ce38d8b47ec364a9eb9@135.181.24.128:28656,7269c0c69310de9f26cb030a7be2e8bd4561997c@143.198.60.85:26656,ff201c380cc1fe22039a627ffbe22ffb594aec46@95.217.1.37:26656,c0eaf7908a62b83e5f7ee3d6ccab207343c15b93@90.188.3.212:26656,bfbd634aed4299b726424c744dee68c0675c88f5@65.108.124.172:29656,a6401a34c3c7bd94ee29e7d2d5c46b0f2c653d44@164.92.218.193:26656,d5c713eb9880e92b1a6cdb2c4804466a32d133d3@161.35.42.118:26656,cf796b322e34d68a276b352222bb7b2293ed953a@217.199.117.158:56656,5d0ada752728ad5dd8c62d9866fdef2b7322cc26@45.79.250.108:26656,1419aab2e0c25d559e5e213e81667af1e96e95f6@45.10.154.127:26656,505061abd18857b22c354bedb3bd755c76b10ba0@157.230.85.65:26656,dc48a2e124a0667504c6f6b74db0511e8ffba516@65.108.68.233:26601,6c1dfe43d9c0c06f639f7deb6c3b8bb3cabc2647@68.183.12.38:26656,23bf0ceca59442aa869ab95e55c573f2a7b6819b@46.101.159.195:26656,58b5a8b2c6c8945cd478668f2e148b0458753ede@178.62.1.156:26656,36a9b03ae5418c296481f1420dae7442050150fb@174.138.1.118:26656,3084dd928a8263277ac69dc0e4f830f702b0da1b@65.109.60.239:34656,c327443ba07332c6d03eaaff81405bf7eb89ec68@194.146.13.229:26656,187239e6534515df119904481b812b52ab0b2a27@209.97.177.123:26656,aa9190840ec2125148c1fcc61b2bcdb01aacce35@188.166.49.152:26656,bb4c7919c1c8cb5d3fed102f6019142f3a71c9f5@77.91.123.193:26656,3384179374755adcadc666170e22f504167fa8e0@38.242.214.172:2456,52f195ea69bb2f4e4c0ffe15a1814ae5ccbc4d8f@38.242.132.159:26656,4d8406189309d6afb008e87f893d35dd10a9a2ec@45.88.223.161:26656,4bb02c1155e43b54b7e458a771afa5f80c8207f0@89.163.231.30:26656,b576762786c937362c7b5884bcbc3774b4df8f60@128.199.49.113:26656,bf5802cfd8688e84ac9a8358a090e99b5b769047@135.181.176.109:53656,604042c495368c0b3513b8541dbf88e217d52cdf@207.180.223.111:26656,8a9395393163fe97ff6a45203fd4a59b5171664a@91.230.110.94:26656,2596ec3b54d2c628ffb6c3f0b43cbd46eadbf11f@65.108.129.29:60656,e39f99ef512beef680e588c363715991485406d9@194.146.13.106:26656,5a4865ccf89affef7a99e83b31f96ac898cfb3ef@159.65.206.7:26656,93afa43fa4ebf0fa7144fd0a15024caac1f4f87e@45.91.170.80:26656,03d719f115066060976adb6e45270d319bc9de21@143.198.150.51:26656,951bae8787569f0c33651edbac40c97afc6ae198@88.255.100.129:26656,4c4258747e1b94826694b8e946707c20d544ab29@137.184.86.70:26656,41d5b172abaf694eb30d05625c6d187b48ad4585@95.216.217.85:26656,0bc82a3608bf73761e05ac592686891a40d90e5a@195.3.222.188:26656,7f3a30c3a7663bb91f1fe16e1eb45dbe91988a4d@178.63.102.172:56656"
sed -i.default "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/;" $HOME/.okp4d/config/config.toml
```

### optimization
```
sed -i.default "s/pruning *=.*/pruning = \"custom\"/g" $HOME/.okp4d/config/app.toml
sed -i "s/pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/g" $HOME/.okp4d/config/app.toml
sed -i "s/pruning-interval *=.*/pruning-interval = \"10\"/g" $HOME/.okp4d/config/app.toml
sed -i -e "s/indexer *=.*/indexer = \"null\"/g" $HOME/.okp4d/config/config.toml
```

### minimum-gas-prices ??? what value
```
minimum-gas-prices = "0.001uknow"
```

### custom ports
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${OKP4_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${OKP4_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${OKP4_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${OKP4_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${OKP4_PORT}660\"%" $HOME/.okp4d/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${OKP4_PORT}317\"%; s%^address = \":8080\"%address = \":${OKP4_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${OKP4_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${MANDE_PORT}091\"%" $HOME/.okp4d/config/app.toml
```

### download genesis
```
wget -O $HOME/.okp4d/config/genesis.json https://raw.githubusercontent.com/okp4/networks/main/chains/nemeton/genesis.json
```

### client config
```
okp4d config chain-id $OKP4_CHAIN_ID
okp4d config keyring-backend test
okp4d config node tcp://localhost:${OKP4_PORT}657
```

### unsafe reset
```
okp4d tendermint unsafe-reset-all
```

### download addrbook
```
wget -O $HOME/.okp4d/config/addrbook.json https://api.nodes.guru/okp4_addrbook.json
```

### create service
```
echo "[Unit]
Description=OKP4 Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/usr/local/bin/okp4d start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/okp4d.service
sudo mv $HOME/okp4d.service /etc/systemd/system
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```

### start node
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable okp4d
sudo systemctl restart okp4d
sudo journalctl -u okp4d -f -o cat
```

### sync status
```
okp4d status | jq .SyncInfo
```
> `cathing_up: true` means still syncing    
> `false` means synced, you can create validator    

> this timestamp, as shown, means that your node is unable to start syncing. Add actual peers to config.toml or repalce addrbook    

![](https://github.com/toolfun/pics/blob/main/1970sync.jpg)

### wallet add
```
okp4d keys add $OKP4_WALLET
```

#### addresses
valoper's
```
okp4d keys show $OKP4_WALLET --bech val -a
```
wallet's
```
okp4d keys show $OKP4_WALLET --bech -a
```

### faucet
https://faucet.okp4.network    
https://discord.com/channels/946759919678406696/1034483599996620820
    
### create validator
```
okp4d tx staking create-validator \
--amount=990000uknow \
--pubkey=$(okp4d tendermint show-validator) \
--moniker="$OKP4_NODENAME" \
--commission-rate="0.06" \
--commission-max-rate="0.25" \
--commission-max-change-rate="0.1" \
--min-self-delegation="1" \
--fees=1000uknow \
--from=$OKP4_WALLET \
-y
```
____

### some comands
#### status
```
curl localhost:24657/status | jq
```

#### add adresses as variables
```
OKP4_WALLET_ADDRESS=$(okp4dd keys show $WALLET -a)
OKP4_VALOPER=$(okp4dd keys show $WALLET --bech val -a)
```
```
echo 'export OKP4_WALLET_ADDRESS='${OKP4_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export OKP4_VALOPER='${OKP4_VALOPER} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

#### delegate
```
okp4d tx staking delegate $OKP4_VALOPER 10000000uknow --from $OKP4_WALLET --fees 5000uknow
```
#### active validators list
```
okp4d query staking validators --limit 2000 -o json | jq -r '.validators[] | select(.status=="BOND_STATUS_BONDED") | [.operator_address, .status, (.tokens|tonumber / pow(10; 6)), .description.moniker] | @csv' | column -t -s"," | sort -k3 -n -r
```
