`# mainnet` `# v0.34.1`


# AXELAR node setup



### System update
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### Go installation
```
ver="1.21.5"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
sudo rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
```

### Binary installation
```
cd
sudo rm -r axelar-core
git clone https://github.com/axelarnetwork/axelar-core.git
cd axelar-core
git checkout v0.34.1 # v0.35.5 on block 11795700 
make build
```
```
mv $HOME/axelar-core/bin/axelard $HOME/go/bin
```

### Customizing with variables (moniker will be visible in blockchain, wallet name is not)
```
moniker=
wallet=
chain=axelar-dojo-1
```

You can customize which ports will be used, or do nothing and leave as default.
My custom port is
```
port=23
```

### Init
```
axelard init $AXELAR_M --chain-id $AXELAR_CHAIN --home $HOME/.axelar
```

### Download genesis
```
wget -O ~/.axelar/config/genesis.json https://snapshots.polkachu.com/genesis/axelar/genesis.json --inet4-only
```

### Address book
```
curl -Ls https://snapshots.kjnodes.com/axelar/addrbook.json > $HOME/.axelar/config/addrbook.json
```

<!-- ####################################### config
### App config
```
axelard config chain-id $AXELAR_CHAIN --home $HOME/.axelar
axelard config node tcp://localhost:${AXELAR_PORT}657 --home $HOME/.axelar
axelard config keyring-backend file --home $HOME/.axelar
```
####################################### config -->

### Minimum gas prices
```
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.007uaxl\"|" $HOME/.axelar/config/app.toml
```

### Pruning settings
```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "50000"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "199"|' \
  $HOME/.axelar/config/app.toml
```

### Ports customizing
```
sed -i.bak \
-e "s%^proxy_app = \"tcp://127.0.0.1:[0-9]\{2\}658\"%proxy_app = \"tcp://127.0.0.1:${AXELAR_PORT}658\"%;" \
-e "s%^laddr = \"tcp://127.0.0.1:[0-9]\{2\}657\"%laddr = \"tcp://127.0.0.1:${AXELAR_PORT}657\"%;" \
-e "s%^pprof_laddr = \"localhost:[0-9]\{1\}060\"%pprof_laddr = \"localhost:${AXELAR_PORT}060\"%;" \
-e "s%^laddr = \"tcp://0.0.0.0:[0-9]\{2\}656\"%laddr = \"tcp://0.0.0.0:${AXELAR_PORT}656\"%;" \
-e "s%^prometheus_listen_addr = \":[0-9]\{2\}660\"%prometheus_listen_addr = \":${AXELAR_PORT}660\"%" \
$HOME/.axelar/config/config.toml

sed -i.bak \
-e "s%^address = \"tcp://0.0.0.0:[0-9]\{1\}317\"%address = \"tcp://0.0.0.0:${AXELAR_PORT}317\"%;" \
-e "s%^address = \":[0-9]\{1\}080\"%address = \":${AXELAR_PORT}080\"%;" \
-e "s%^address = \"0.0.0.0:[0-9]\{1\}090\"%address = \"0.0.0.0:${AXELAR_PORT}090\"%;" \
-e "s%^address = \"0.0.0.0:[0-9]\{1\}091\"%address = \"0.0.0.0:${AXELAR_PORT}091\"%;" \
$HOME/.axelar/config/app.toml
```

### Service file
```
sudo tee /etc/systemd/system/axelard.service > /dev/null <<EOF
[Unit]
Description=Axelar_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which axelard) start --home $HOME/.axelar
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Add seed node
```
sed -i 's/seeds = ""/seeds = "ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:15156"/' ~/.axelar/config/config.toml
```

### Reset
```
axelard tendermint unsafe-reset-all --home $HOME/.axelar --keep-addr-book
```

### Sync
**kjnodes**    
**polkachu**    
`...`    
```
sudo systemctl daemon-reload
sudo systemctl start axelard && sudo journalctl -u axelard -f --no-hostname -o cat
```

