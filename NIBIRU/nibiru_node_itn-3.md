Nibiru Incentivized Testnet (NIT). 4 phases, March - Q4 2023    

# NIBIRU `nibiru-itn-3`

### Final 4th phase (NIT4)
NIT1 - Oracles and Staking    
NIT2 - Governance    
NIT3 - Liquidity Applications Perps & Spot    
👉 NIT4 - Trading competition    

### ❗ for this phase is not nessesary to install a node, this phase is about trading competition
https://nibiru.fi/blog/posts/024-itn-4.html
____


### Updating
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### Install Go
```
sudo rm -rf /usr/local/go
v="1.21.0"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```

### Customizing
Set variables `moniker` for your validator node, `wallet` name. The `port` can be changed, `chain` is not.
```
moniker=
wallet=
port=17
chain=nibiru-itn-3
```

Load variables
```
echo "export NIBIRU_M=$moniker" >> $HOME/.bash_profile
echo "export NIBIRU_W=$wallet" >> $HOME/.bash_profile
echo "export NIBIRU_CHAIN=$chain" >> $HOME/.bash_profile
echo "export NIBIRU_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Install the Nibiru binary
```
curl -s https://get.nibiru.fi/@v0.21.11! | bash
```

### Init the Nibiru chain
```
nibid init $NIBIRU_M --chain-id=$NIBIRU_CHAIN
```

### Download genesis
```
curl -s https://rpc.itn-3.nibiru.fi/genesis | jq -r .result.genesis > $HOME/.nibid/config/genesis.json
```

### Config Nibiru
```
nibid config chain-id $NIBIRU_CHAIN
nibid config keyring-backend file
nibid config node tcp://localhost:${NIBIRU_PORT}657
```

### Customizing ports (optional)
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${NIBIRU_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${NIBIRU_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${NIBIRU_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${NIBIRU_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${NIBIRU_PORT}660\"%" $HOME/.nibid/config/config.toml
sed -i.bak -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:${NIBIRU_PORT}317\"%; s%^address = \":8080\"%address = \":${NIBIRU_PORT}080\"%; s%^address = \"localhost:9090\"%address = \"localhost:${NIBIRU_PORT}090\"%; s%^address = \"localhost:9091\"%address = \"localhost:${NIBIRU_PORT}091\"%" $HOME/.nibid/config/app.toml
```

### Set minimum-gas-prices
```
sed -i 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025unibi"/g' $HOME/.nibid/config/app.toml

```

### Set pruning (optional)
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.nibid/config/app.toml
```

### Turn off indexer (optional)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.nibid/config/config.toml
```

### Service file
```
sudo tee /etc/systemd/system/nibid.service > /dev/null <<EOF
[Unit]
Description=Nibiru_ITN-3
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nibid) start --home $HOME/.nibid
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Snapshot. Using kjnode's one
```
sudo systemctl stop nibid
cp $HOME/.nibid/data/priv_validator_state.json $HOME/.nibid/priv_validator_state.json.backup
rm -rf $HOME/.nibid/data
curl -L https://snapshots.kjnodes.com/nibiru-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.nibid
mv $HOME/.nibid/priv_validator_state.json.backup $HOME/.nibid/data/priv_validator_state.json
sudo systemctl start nibid && sudo journalctl -u nibid -f --no-hostname -o cat
```

### Start the Nibiru node
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable --now nibid
journalctl -u nibid -f -o cat
```

### Add new wallet
```
nibid keys add $NIBIRU_W
```
or recover wallet
```
nibid keys add $NIBIRU_W --recover
```

### Faucet
[Discord Nibiru](https://discord.gg/F38Q37Sj) #faucet       
[app.nibiru.fi](https://app.nibiru.fi) 

### Create validator
```
nibid tx staking create-validator \
--amount=1000000unibi \
--pubkey=$(nibid tendermint show-validator) \
--moniker="$NIBIRU_M" \
--commission-rate="0.05" \
--commission-max-rate="0.20" \
--commission-max-change-rate="0.1" \
--min-self-delegation="1000000" \
--from=$NIBIRU_W \
--fees=5000unibi \
-y
```

### Add wallet and valoper addresses as variables
```
NIBIRU_WA=$(nibid keys show $NIBIRU_W -a)
NIBIRU_V=$(nibid keys show $NIBIRU_W --bech val -a)
```
```
echo 'export NIBIRU_WA='\"${NIBIRU_WA}\" >> $HOME/.bash_profile
echo 'export NIBIRU_V='\"${NIBIRU_V}\" >> $HOME/.bash_profile
source $HOME/.bash_profile
```


____

# Oracle price feeder

### install price feeder
```
curl -s https://get.nibiru.fi/pricefeeder@v0.21.6! | bash
```

### add new wallet for the price feeder
```
nibid keys add pf_wallet
```

### feeder wallet must have tokens to work
Use [faucet](https://github.com/toolfun/_nets/blob/main/NIBIRU/nibiru_node_itn-3.md#faucet) or send to feeder wallet address

### set variables
```bash
export GRPC_ENDPOINT="localhost:${NIBIRU_PORT}090"
export WEBSOCKET_ENDPOINT="ws://localhost:${NIBIRU_PORT}657/websocket"
export EXCHANGE_SYMBOLS_MAP='{"bitfinex":{"ubtc:unusd":"tBTCUSD","ubtc:uusd":"tBTCUSD","ueth:unusd":"tETHUSD","ueth:uusd":"tETHUSD","uusdc:uusd":"tUDCUSD","uusdc:unusd":"tUDCUSD"},"coingecko":{"ubtc:uusd":"bitcoin","ubtc:unusd":"bitcoin","ueth:uusd":"ethereum","ueth:unusd":"ethereum","uusdt:uusd":"tether","uusdt:unusd":"tether","uusdc:uusd":"usd-coin","uusdc:unusd":"usd-coin","uatom:uusd":"cosmos","uatom:unusd":"cosmos","ubnb:uusd":"binancecoin","ubnb:unusd":"binancecoin","uavax:uusd":"avalanche-2","uavax:unusd":"avalanche-2","usol:uusd":"solana","usol:unusd":"solana","uada:uusd":"cardano","uada:unusd":"cardano"}}'
export FEEDER_MNEMONIC="<mnemonic of the added feeder wallet>"
export VALIDATOR_ADDRESS="$NIBIRU_V"
```

### service for the price feeder
```
sudo tee /etc/systemd/system/pricefeeder.service<<EOF
[Unit]
Description=Nibiru Pricefeeder
Requires=network-online.target
After=network-online.target

[Service]
Type=exec
User=$USER
ExecStart=/usr/local/bin/pricefeeder
Restart=on-failure
ExecReload=/bin/kill -HUP $MAINPID
KillSignal=SIGTERM
PermissionsStartOnly=true
LimitNOFILE=65535
Environment=CHAIN_ID='$NIBIRU_CHAIN'
Environment=GRPC_ENDPOINT='$GRPC_ENDPOINT'
Environment=WEBSOCKET_ENDPOINT='$WEBSOCKET_ENDPOINT'
Environment=EXCHANGE_SYMBOLS_MAP='$EXCHANGE_SYMBOLS_MAP'
Environment=FEEDER_MNEMONIC='$FEEDER_MNEMONIC'
Environment=VALIDATOR_ADDRESS='$VALIDATOR_ADDRESS'

[Install]
WantedBy=multi-user.target
EOF
```

### set price feeder wallet
```
nibid tx oracle set-feeder $(nibid keys show pf_wallet --bech -a) --from $NIBIRU_W --fees 5000unibi -y
```

### price feeder wallet must have tokens to work
Get tokens via [faucet](https://github.com/toolfun/_nets/edit/main/NIBIRU/nibiru_node_itn-3.md#faucet) or send tokens to price feeder wallet address    
Show the price feeder wallet address
```
nibid show pf_wallet --bech -a
```

### start price feeder
```
sudo systemctl daemon-reload
sudo systemctl enable --now pricefeeder
journalctl -u pricefeeder -f -o cat
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

