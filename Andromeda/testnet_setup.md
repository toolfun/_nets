# Andromeda
### 🚧🚧🚧 under construction

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
```
ANDROMEDA_M=xAlex
ANDROMEDA_W=wAlex
ANDROMEDA_PORT=15
```
```
ANDROMEDA_CHAIN=galileo-3
```
```
echo "export ANDROMEDA_NODENAME=$ANDROMEDA_NODENAME" >> $HOME/.bash_profile
echo "export ANDROMEDA_WALLET=$ANDROMEDA_WALLET" >> $HOME/.bash_profile
echo "export ANDROMEDA_CHAIN_ID=$ANDROMEDA_CHAIN_ID" >> $HOME/.bash_profile
echo "export ANDROMEDA_PORT=$ANDROMEDA_PORT" >> $HOME/.bash_profile
```
```
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

### Init
```
andromedad init "$ANDROMEDA_NODENAME" --chain-id $ANDROMEDA_CHAIN_ID
```

### Config
```
andromedad config keyring-backend file
andromedad config chain-id galileo-3
andromedad config node tcp://localhost:${ANDROMEDA_PORT}657
```

### Genesis
```
wget -O $HOME/.andromedad/config/genesis.json "https://raw.githubusercontent.com/obajay/nodes-Guides/main/AndromedaProtocol/genesis.json"
```

### Wallet
```
andromedad keys add $ANDROMEDA_WALLET
```

###
```

```

###
```

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
pruning_interval="49"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.andromedad/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.andromedad/config/app.toml
```

### Set minimum gas price
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0uandr\"/;" ~/.andromedad/config/app.toml
```

### Addrbook
```
wget -O $HOME/.andromedad/config/addrbook.json "https://raw.githubusercontent.com/obajay/nodes-Guides/main/AndromedaProtocol/addrbook.json"
```

### Service
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

### State sync / snapshot
```

```

###
```
sudo systemctl daemon-reload
sudo systemctl enable andromedad
sudo systemctl restart andromedad && sudo journalctl -u andromedad -f -o cat
```

### Faucet
Discord


### Create validator
```

```

###
```

```

###
```

```

