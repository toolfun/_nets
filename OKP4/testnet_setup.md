## TESTNET now for the 150 "druids". Guide is not relevant
# 🚧 outdated
____

### ~~OKP4 Setup~~

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
  ver="1.19.4"
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
OKP4_CHAIN_ID=okp4-nemeton-1
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
cd okp4d && git checkout v3.0.0
make install
```

### init
```
okp4d init $OKP4_NODENAME --chain-id=OKP4_CHAIN_ID
```

### add seed and peers 

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
