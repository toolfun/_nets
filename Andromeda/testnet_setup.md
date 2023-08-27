# Andromeda. Installation guide
- Docs: https://docs.andromedaprotocol.io/andromeda-chain/node-and-validator-setup
- Explorer: https://explorer.kjnodes.com/andromeda-testnet
- Explorer: https://andromeda.explorers.guru
- Discord https://discord.gg/6UzCWn84cw

____

### Server update
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### Increasing open files limit
```
sudo su -c "echo 'fs.file-max = 65536' >> /etc/sysctl.conf"
sudo sysctl -p
```

### Go installation
https://go.dev/doc/install    
You can change version under the `v` variable
```
sudo rm -rf /usr/local/go
v="1.21.0"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```
Create `.bash_profile` file and load variables
```
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
```

### Variables
Think up and write a moniker (nodename), wallet name and change port
```
moniker=
wallet=
port=15
chain=galileo-3
```
```
echo "export ANDROMEDA_M=$moniker" >> $HOME/.bash_profile
echo "export ANDROMEDA_W=$wallet" >> $HOME/.bash_profile
echo "export ANDROMEDA_CHAIN=$chain" >> $HOME/.bash_profile
echo "export ANDROMEDA_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Install andromeda
```
cd $HOME
git clone https://github.com/andromedaprotocol/andromedad.git
cd andromedad
git checkout galileo-3-v1.1.0-beta1 
make install
```
> version: galileo-3-v1.1.0-beta1    
> commit: b3f8d880dfcdb3265d321e465b47b04071d9480f

### Init the node
```
andromedad init "$ANDROMEDA_M" --chain-id $ANDROMEDA_CHAIN
```

### Node config
```
andromedad config keyring-backend test
andromedad config chain-id galileo-3
andromedad config node tcp://localhost:${ANDROMEDA_PORT}657
```

### Download genesis
```
wget -O $HOME/.andromedad/config/genesis.json "https://github.com/andromedaprotocol/testnets/raw/galileo-3/genesis.json"
```

### Config indexer (off) *optional*
```
indexer="null" sed -i -e "s/^indexer =./indexer = "$indexer"/" $HOME/.andromedad/config/config.toml
```

### Config custom ports *optional*
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${ANDROMEDA_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${ANDROMEDA_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${ANDROMEDA_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${ANDROMEDA_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${ANDROMEDA_PORT}660\"%" $HOME/.andromedad/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${ANDROMEDA_PORT}317\"%; s%^address = \":8080\"%address = \":${ANDROMEDA_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${ANDROMEDA_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${ANDROMEDA_PORT}091\"%" $HOME/.andromedad/config/app.toml
```

### Config prunings *optional*
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.andromedad/config/app.toml
```

### Set minimum gas price
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0uandr\"/;" ~/.andromedad/config/app.toml
```

### Addrbook (kjnodes)
```
curl -Ls https://snapshots.kjnodes.com/andromeda-testnet/addrbook.json > $HOME/.andromedad/config/addrbook.json
```

### Service file
```
sudo tee /etc/systemd/system/andromedad.service > /dev/null <<EOF
[Unit]
Description=Andromeda_Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which andromedad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Snapshot (kjnodes)
```
curl -L https://snapshots.kjnodes.com/andromeda-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.andromedad
```

### Starting
```
sudo systemctl daemon-reload
sudo systemctl enable --now andromedad && sudo journalctl -u andromedad -f -o cat
```

### Add new or existing wallet
New
```
andromedad keys add $ANDROMEDA_W
```
Existing
```
andromedad keys add $ANDROMEDA_W --recover
```

### Faucet
Discord https://discord.gg/6UzCWn84cw


### Create validator
```
andromedad tx staking create-validator \
  --amount=1000000 uandr \
  --pubkey=$(andromedad tendermint show-validator) \
  --moniker="$ANDROMEDA_M" \
  --commission-rate="0.05" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --gas="700000" \
  --from=$ANDROMEDA_W
```

###
```

```

