### 🚧🚧🚧 under construction
# Arkeo

### updating
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### go installing
```
sudo rm -rf /usr/local/go
v="1.21.0"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```

### customizing with variables
Set variables: `moniker` for your validator node, `wallet` for wallet name. The `chain` leave as is.
```
moniker=
wallet=
chain=arkeo
```
You can customize which ports will be used, or do nothing and leave as default.    
I configure as follows
```
port=18
```

### load variables
```
echo "export ARKEO_M=$moniker" >> $HOME/.bash_profile
echo "export ARKEO_W=$wallet" >> $HOME/.bash_profile
echo "export ARKEO_CHAIN=$chain" >> $HOME/.bash_profile
echo "export ARKEO_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### installing Arkeo
```
cd
git clone https://github.com/arkeonetwork/arkeo
cd arkeo
git checkout ab05b124336ace257baa2cac07f7d1bfeed9ac02
make proto-gen install
arkeod version
```

### config
```
arkeod config chain-id $ARKEO_CHAIN
arkeod config node tcp://localhost:${ARKEO_PORT}657
arkeod config keyring-backend file
```

### init
```
arkeod init ARKEO_M --chain-id $ARKEO_CHAIN
```

### genesis
```
curl -s http://seed.arkeo.network:26657/genesis | jq '.result.genesis' > ~/.arkeo/config/genesis.json
```

### config
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.001uarkeo\"/;" ~/.arkeo/config/app.toml
```

### ports (optional)
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${ARKEO_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${ARKEO_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${ARKEO_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${ARKEO_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${ARKEO_PORT}660\"%" $HOME/.arkeo/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${ARKEO_PORT}317\"%; s%^address = \":8080\"%address = \":${ARKEO_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${ARKEO_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${ARKEO_PORT}091\"%" $HOME/.arkeo/config/app.toml
```

### customize pruning (optional)
```
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
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.arkeo/config/config.toml
```

### add seed
```
seeds="20e1000e88125698264454a884812746c2eb4807@seeds.lavenderfive.com:22856"
sed -i 's|^seeds *=.*|seeds = "'$seeds'"|' $HOME/.arkeo/config/config.toml
```

### addrbook
```

```

### reset
```
arkeod tendermint unsafe-reset-all --home $HOME/.arkeo --keep-addr-book
```

### service arkeod
```
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
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable --now arkeod
journalctl -u arkeod -f -o cat
```

### add account
```
arkeod keys add $ARKEO_W
```

### faucet
> Ask in Arkeo [Discord](https://discord.gg/BfEHpm6uFc)
> [form](https://docs.google.com/forms/d/e/1FAIpQLSeBNEWdmTRGG_UWMj5HxUeQB141rhW9T6teOTnzMP_6mXAzMA/viewform)

### create validator
```
arkeod tx staking create-validator \
--commission-rate 0.05 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.1 \
--min-self-delegation "1" \
--amount 1000000uarkeo \
--pubkey $(arkeod tendermint show-validator) \
--moniker "$ARKEO_M" \
--from "$ARKEO_W" \
--fees="5000uarkeo" \
-y
```

### edit validator
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

###
```

```

###
```

```

###
```

```

###
```

```

###
```

```
