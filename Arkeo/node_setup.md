### `arkeo-testnet-3`
### `validator setup`

#

# Arkeo

### updating
```bash
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### go installing
```bash
sudo rm -rf /usr/local/go
v="1.21.13"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```

### customizing with variables
Set variables: `moniker` for your validator node, `wallet` for wallet name. The `chain` leave as is.
```bash
moniker=
wallet=
chain=arkeo-testnet-3
```
You can customize which ports will be used, or do nothing and leave as default.    
For example set
```
port=18
```

### load variables
```bash
echo "export ARKEO_M=$moniker" >> $HOME/.bash_profile
echo "export ARKEO_W=$wallet" >> $HOME/.bash_profile
echo "export ARKEO_CHAIN=$chain" >> $HOME/.bash_profile
echo "export ARKEO_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### installing Arkeo
```bash
cd
git clone https://github.com/arkeonetwork/arkeo
cd arkeo
git checkout master
TAG=testnet make install
arkeod version
```

### config
```bash
arkeod config set client chain-id $ARKEO_CHAIN
arkeod config set client keyring-backend test
arkeod config set client node tcp://localhost:${ARKEO_PORT}657
```

### init
```bash
arkeod init ARKEO_M --chain-id $ARKEO_CHAIN
```

### genesis
```bash
curl -s http://seed31.innovationtheory.com:26657/genesis | jq '.result.genesis' > $HOME/.arkeo/config/genesis.json
```

### config minimum gas prices
```bash
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001uarkeo\"/;" ~/.arkeo/config/app.toml
```

### ports (optional)
```bash
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${ARKEO_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${ARKEO_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${ARKEO_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${ARKEO_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${ARKEO_PORT}660\"%" $HOME/.arkeo/config/config.toml
sed -i.bak -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:${ARKEO_PORT}317\"%; s%^address = \"localhost:9090\"%address = \"localhost:${ARKEO_PORT}090\"%" $HOME/.arkeo/config/app.toml
```

### customize pruning (optional)
```bash
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.arkeo/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.arkeo/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.arkeo/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.arkeo/config/app.toml
```

### indexer off (optional)
```bash
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.arkeo/config/config.toml
```

### add seed
```bash
seeds="9dfa5f2d19c1174baf5e597965394269e654f9b7@seed31.innovationtheory.com:26656"
sed -i 's|^seeds *=.*|seeds = "'$seeds'"|' $HOME/.arkeo/config/config.toml
peers="bb761c984bd990f3055f412917396754cd22af7a@validator31.innovationtheory.com:26656,81e36f94351d47803b8e1e0d0ad2d2e8e14ed36b@validator32.innovationtheory.com:26656"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.arkeo/config/config.toml
```

### addrbook
```

```

### reset
```
arkeod tendermint unsafe-reset-all --home $HOME/.arkeo --keep-addr-book
```

### service arkeod
```bash
sudo tee /etc/systemd/system/arkeod.service > /dev/null <<EOF
[Unit]
Description=Arkeo_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which arkeod) start --home $HOME/.arkeo
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### start
```bash
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable --now arkeod
journalctl -u arkeod -f -o cat
```

### add account
```bash
arkeod keys add $ARKEO_W
```

### faucet
> Ask in Arkeo [Discord](https://discord.gg/BfEHpm6uFc)
> [form](https://docs.google.com/forms/d/e/1FAIpQLSeBNEWdmTRGG_UWMj5HxUeQB141rhW9T6teOTnzMP_6mXAzMA/viewform)

### create validator
1. Load a pubkey into variable
```bash
PUBKEY=$(arkeod comet show-validator)
```

2. Create a json file with all the specifications of your validator
```bash
nano $HOME/.arkeo/validator.json
```
Ready-to-go example. Check if it fits your needs
> commission-max-rate and commission-max-change-rate can't be changed after setup     
```bash
cat <<EOF > $HOME/.arkeo/validator.json
{
        "pubkey": $PUBKEY,
        "amount": "100000000uarkeo", 
        "moniker": $ARKEO_M,
        "identity": "",
        "website": "",
        "details": "",
        "commission-rate": "0.1",
        "commission-max-rate": "0.2",
        "commission-max-change-rate": "0.01",
        "min-self-delegation": "1"
}
EOF
```

2. Run the create validator command
```
arkeod tx staking create-validator $HOME/.arkeo/validator.json \
--from=$ARKEO_W \
--chain-id=arkeo-testnet-3 \
--fees=200uarkeo \
--yes
```

### Optional. How to edit validator parameters
> to add or remove description on your validator, for example adding website and your logo
```bash
arkeod tx staking edit-validator \
--from=ARKEO_W \
--website=<YOUR_LINK> \
--identity=<KEYBASE FINGERPRINT> \
-y
```


###
```

```
