### `chiado_10010-1`

#### `node setup` `validator setup` `oracle setup`

# Warden Protocol

### Installing tools
```bash
sudo apt update
sudo apt install make gcc curl wget git jq build-essential lz4 unzip -qy
sudo apt upgrade
```

### Install Go 1.22.3 or later
```bash
sudo rm -rf /usr/local/go
v="1.22.3"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```

### Set variables: moniker for your validator node, wallet for wallet name. The chain leave as is
```bash
moniker=
wallet=
chain="chiado_10010-1"
```

### You can customize which ports will be used, or do nothing and leave as default.
For example set
```bash
port=15
```

### Load variables
```bash
echo "export WARDEN_MONIKER=$moniker" >> $HOME/.bash_profile
echo "export WARDEN_WALLET=$wallet" >> $HOME/.bash_profile
echo "export WARDEN_CHAIN=$chain" >> $HOME/.bash_profile
echo "export WARDEN_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Getting Warden binary
```bash
mkdir -p ~/wardenprotocol && cd ~/wardenprotocol
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.5.2/wardend_Linux_x86_64.zip
unzip wardend_Linux_x86_64.zip
chmod +x wardend
mv $HOME/wardenprotocol/wardend $HOME/go/bin
```

### Check version
```bash
wardend version --long | grep -e version -e commit
```

### Init
```bash
wardend init $WARDEN_MONIKER --chain-id $WARDEN_CHAIN
```

### Config app
```bash
wardend config set client chain-id $WARDEN_CHAIN
```

### Configure keyring-backend
> keyring-backend test - no password required to set    
> keyring-backend os - setting password required    
> keyring-backend file - setting password required    
Choose your backend store 
```bash
wardend config set client keyring-backend test
```

### Download Genesis file
```bash
wget -O $HOME/.warden/config/genesis.json "https://raw.githubusercontent.com/warden-protocol/networks/main/testnets/chiado/genesis.json"
```

### Address books
You can find addrbooks or ask for it, if you have issues with syncing, in Warden Protocol Discord https://discord.gg/wardenprotocol    
- itrocket.net
```bash
wget -O $HOME/.warden/config/addrbook.json https://server-4.itrocket.net/testnet/warden/addrbook.json
```
- UTSA
```bash
wget -O $HOME/.warden/config/addrbook.json "https://share.utsa.tech/warden/addrbook.json"
```

### Replacing default ports to avoid app conflicts (optional)
```bash
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${WARDEN_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${WARDEN_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${WARDEN_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${WARDEN_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${WARDEN_PORT}660\"%" $HOME/.warden/config/config.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${WARDEN_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WARDEN_PORT}091\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${WARDEN_PORT}327\"%" $HOME/.warden/config/app.toml
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:${WARDEN_PORT}657\"%" $HOME/.warden/config/client.toml
```

### Set pruning to reduce filling up disk space (optional)
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.warden/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"17\"/" $HOME/.warden/config/app.toml
```

### Disable indexer filling up disk space (optional)
```bash
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.warden/config/config.toml
```

### Setting minimum gas price
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "25000000award"|g' $HOME/.warden/config/app.toml
```

### Create service file fo wardend
```bash
sudo tee /etc/systemd/system/wardend.service > /dev/null <<EOF
[Unit]
Description=Wardend_testnet_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which wardend) start
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Reset chain
```bash
wardend tendermint unsafe-reset-all --home $HOME/.warden
```

### Download snapshot
You can find a snapshot or ask for it, to reduce time for syncing, in Warden Protocol Discord https://discord.gg/wardenprotocol

#### In case we will run warden price oracle, update the `Oracle` section of the app.toml file with the settings
```bash
sed -i.bak -e 's/price_ttl = "0s"/price_ttl = "10s"/' -e 's/interval = "0s"/interval = "5s"/' $HOME/.warden/config/app-test.toml
```

### Enable and start service
```bash
sudo systemctl daemon-reload
sudo systemctl enable wardend
sudo systemctl restart wardend && sudo journalctl -u wardend -o cat -f
```

> Create an account (wallet) while node is syncing

### Create wallet
```bash
wardend keys add $WARDEN_WALLET
```

### Or restore wallet using mnemonic
```bash
wardend keys add $WARDEN_WALLET --recover
```

### Save and load account addresses, wallet and validator operator
```bash
WARDEN_WALLET_ADDR=$(wardend keys show $WARDEN_WALLET -a)
WARDEN_VALOPER=$(wardend keys show $WARDEN_WALLET --bech val -a)
echo "export WARDEN_WALLET_ADDR="$WALLET_ADDR >> $HOME/.bash_profile
echo "export WARDEN_VALOPER="$VALOPER >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Create validator
> #### You can create a validator only after full synchronization
> #### Check status
```bash
wardend status 2>&1 | jq .sync_info
```

> If `"catching_up": false`, you can now create a validator    
Load the pubkey into the variable
```bash
PUBKEY=$(wardend tendermint show-validator)
```

#### Create validator specs file
```bash
nano $HOME/.warden/validator.json
```
#### Paste/edit to it your validator specs
```bash
{
  "pubkey": "$PUBKEY",
  "amount": "1000000000000000000award",
  "moniker": "WARDEN_MONIKER",
  "identity": "",
  "website": "",
    "details": "",
  "commission-rate": "0.07",
  "commission-max-rate": "0.2",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}
```

#### Creating validator command
```bash
wardend tx staking create-validator $HOME/.warden/validator.json \
 --from $WARDEN_WALLET \
 --gas-prices=25000000award \
 --gas auto \
 --gas-adjustment 1.2 \
 -y
```

## Warden Oracle setup

### Install slinky
```bash
cd
git clone https://github.com/skip-mev/connect.git
cd connect
git checkout v1.0.5
make install
```

### Create service
```bash
sudo tee /etc/systemd/system/slinky.service > /dev/null <<EOF
[Unit]
Description=Warden_price_oracle
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/connect
ExecStart=$(which slinky) --market-map-endpoint 0.0.0.0:9090
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

### Enable autostart and run oracle service
```bash
sudo systemctl enable --now slinky && sudo journalctl -u slinky -f -o cat
```
