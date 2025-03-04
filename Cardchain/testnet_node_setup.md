<!--### 🚧🚧🚧 under construction -->
`cardtestnet-13`

### DecentralCardGame - Crowd Control
## Testnet node installation
- Docs: https://github.com/DecentralCardGame/Testnet
- Explorer: https://explorer.stavr.tech/cardchain
- Explorer: https://explorers.acloud.pp.ua/cardchain
- Discord: https://discord.gg/xvuEfcaUd6

____


### Updating
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential ncdu -qy
sudo apt upgrade
```

### Go installation
https://go.dev/doc/install    
```
sudo rm -rf /usr/local/go
v="1.21.5"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
```

<!-- ##############################################
Create .bash_profile file and update $PATH
```
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
```
############################################## -->


### Set variables
Think up and write a `moniker` (nodename), name of the `wallet` and change `port` (you can put your own number). Keep the `chain` intact 
```
moniker=
wallet=
port=11
chain=cardtestnet-13
```

### Write and load variables
```
echo "export CC_M=$moniker" >> $HOME/.bash_profile
echo "export CC_W=$wallet" >> $HOME/.bash_profile
echo "export CC_CHAIN=$chain" >> $HOME/.bash_profile
echo "export CC_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Download CC binary
```
ccver='v0.17.0'
sudo wget https://github.com/DecentralCardGame/Cardchain/releases/download/$ccver/cardchaind -O $HOME/go/bin/cardchaind
sudo chmod 775 $HOME/go/bin/cardchaind
```

### Set config and init
```
cardchaind config chain-id $CC_CHAIN
cardchaind config keyring-backend test
cardchaind config node tcp://localhost:${CC_PORT}657
cardchaind init $CC_M --chain-id $CC_CHAIN
```

### Download genesis
```
wget https://cardchain.crowdcontrol.network/files/genesis.json -O ~/.cardchaind/config/genesis.json
```

### Config indexer (off) *optional*
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.cardchaind/config/config.toml
```

### Gas-prices
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0ubpf\"/;" ~/.cardchaind/config/app.toml
```

### Changing custom ports *optional*
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CC_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CC_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CC_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CC_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CC_PORT}660\"%" $HOME/.cardchaind/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CC_PORT}317\"%; s%^address = \":8080\"%address = \":${CC_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CC_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CC_PORT}091\"%" $HOME/.cardchaind/config/app.toml
```

### Config prunings *optional*
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.cardchaind/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.cardchaind/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.cardchaind/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.cardchaind/config/app.toml
```

### Add peer
```
PEERS="86fe149f801ac75213179be5b56fbd1a1e545c43@202.61.225.157:20056"
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.cardchaind/config/config.toml
```

### Service file for cardchaind
```
sudo tee /etc/systemd/system/cardchaind.service > /dev/null <<EOF
[Unit]
Description=Cardchain_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cardchaind) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Addressbook, sync
Look for in the Crowd Control Discord https://discord.gg/xvuEfcaUd6 or ask the community

### Reset
```
cardchaind tendermint unsafe-reset-all --home $HOME/.cardchaind --keep-addr-book
```

### Start Cardchain
```
sudo systemctl daemon-reload
sudo systemctl enable --now cardchaind && sudo journalctl -u cardchaind -f -o cat
```

### Add new or existing wallet
New
```
cardchaind keys add $CC_W
```
Existing
```
cardchaind keys add $CC_W --recover
```

### Create validator
```
cardchaind tx staking create-validator \
  --from=$CC_W \
  --amount=1000000ubpf \
  --moniker=$CC_M \
  --chain-id=$CC_CHAIN \
  --commission-rate=0.1 \
  --commission-max-rate=0.5 \
  --commission-max-change-rate=0.1 \
  --min-self-delegation=1 \
  --pubkey=$(cardchaind tendermint show-validator) \
  --yes
```

____


### Delete Cardchain node
```bash
# Deleting service
sudo systemctl stop cardchaind
sudo rm /etc/systemd/system/cardchaind.service

# Deleting work folder folder
sudo rm -r $HOME/.cardchaind/

# Deleting binary
sudo rm /usr/local/bin/cardchaind
```
