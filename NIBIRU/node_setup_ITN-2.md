Nibiru Incentivized Testnet (NIT). 4 phases, March - Q4 2023
# NIBIRU `nibiru-itn-2`. Active
### Final 4th phase (NIT4)
NIT1 - Oracles and Staking    
NIT2 - Governance    
NIT3 - Liquidity Applications Perps & Spot    
👉 NIT4 - Trading competition    
____


### Updating
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### Create .bash_profile
```
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
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
chain=nibiru-itn-2
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
curl -s https://get.nibiru.fi/@v0.21.9! | bash
```

### Init the Nibiru chain
```
nibid init $NIBIRU_M --chain-id=$NIBIRU_CHAIN
```

### Download genesis
```
NETWORK=nibiru-itn-2
curl -s https://networks.itn2.nibiru.fi/$NETWORK/genesis > $HOME/.nibid/config/genesis.json
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
Description=Nibiru_ITN-2_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nibid) start --home $HOME/.nibid
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
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

