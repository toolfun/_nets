
![](https://github.com/toolfun/_pics/blob/eaed8ab851a278e3bf5c59b0f38e09932dc10b1a/marsprotocol.jpg)
### Mars testnet setup guide

- Website: https://marsprotocol.io/
- Network: ares-1
- Explorer official: https://testnet-explorer.marsprotocol.io/
- Explorer NG: https://mars.explorers.guru/
- Faucet: https://faucet.marsprotocol.io/
#

### prepare server
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl tar wget clang pkg-config libssl-dev build-essential jq git make ncdu gcc chrony screen htop lz4 -y
```

### go
```
if ! [ -x "$(command -v go)" ]; then
  ver="1.19.4"
  cd $HOME
  wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
  sudo rm "go$ver.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
fi
```

### set your variables e.g. MARS_PORT=23 MARS_WALLET=anyname

`MARS_NODENAME=`    
`MARS_PORT=`    
`MARS_WALLET=`

```
echo "export MARS_NODENAME=$MARS_NODENAME" >> $HOME/.bash_profile
echo "export MARS_WALLET=$MARS_WALLET" >> $HOME/.bash_profile
echo "export MARS_CHAIN_ID=$MARS_CHAIN_ID" >> $HOME/.bash_profile
echo "export MARS_PORT=$MARS_PORT" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### clone and install
```
git clone https://github.com/mars-protocol/hub mars
cd mars
git checkout v1.0.0-rc7
make install
```

### check version
`marsd version --long | grep -e version -e commit`
> version: 1.0.0-rc7
> commit: 4cc5d7e7aa4e92a117e283b08221eb4c285bf141

### config
```
marsd config chain-id ares-1
marsd config keyring-backend file
marsd config node tcp://localhost:$MARS_PORT
```

### node init
```
marsd init $MARS_NODENAME --chain-id $MARS_CHAIN_ID
```

### custom ports
```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:23658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:23657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:23060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:23656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":23660\"%" $HOME/.mars/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:23317\"%; s%^address = \":8080\"%address = \":23080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:23090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:23091\"%; s%^address = \"0.0.0.0:8545\"%address = \"0.0.0.0:23545\"%; s%^ws-address = \"0.0.0.0:8546\"%ws-address = \"0.0.0.0:23546\"%" $HOME/.mars/config/app.toml
```

### download genesis
```
wget -O $HOME/.mars/config/genesis.json https://raw.githubusercontent.com/mars-protocol/networks/main/ares-1/genesis.json
```

### config pruning (optional)
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="17"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.mars/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.mars/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.mars/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.mars/config/app.toml
```
```
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0umars\"|" $HOME/.mars/config/app.toml
```

### disable indexing (optional)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.mars/config/config.toml
```

### create service
```
sudo tee /etc/systemd/system/marsd.service > /dev/null << EOF
[Unit]
Description=Mars testnet node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which marsd) start
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable marsd
```
```
marsd tendermint unsafe-reset-all --home $HOME/.mars
```

### snapshot (kjnodes)
```
sudo systemctl stop marsd
cp $HOME/.mars/data/priv_validator_state.json $HOME/.mars/priv_validator_state.json.backup
rm -rf $HOME/.mars/data
```
```
curl -L https://snapshots.kjnodes.com/mars-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.mars
mv $HOME/.mars/priv_validator_state.json.backup $HOME/.mars/data/priv_validator_state.json
```

### adrrbook
```
wget -O $HOME/.mars/config/addrbook.json https://raw.githubusercontent.com/toolfun/_nets/main/Mars/addrbook.json
```

### start node
```
sudo systemctl restart marsd && sudo journalctl -u marsd -f --no-hostname -o cat
```

### add wallet
```
marsd keys add $MARS_WALLET
```

##### or recover wallet
```
marsd keys add $MARS_WALLET --recover
```

### create validator
```
marsd tx staking create-validator \
--amount=1000000umars \
--pubkey=$(marsd tendermint show-validator) \
--moniker="$MARS_NODENAME" \
--identity="" \
--details="" \
--website="" \
--commission-rate=0.07 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.03 \
--min-self-delegation=1 \
--from="$MARS_NODENAME"
-y
```

### add walllet and valoper addresses
```
MARS_WALLET_ADDRESS=$(haqqd keys show $MARS_WALLET -a)
MARS_VALOPER=$(haqqd keys show $MARS_WALLET --bech val -a)
echo 'export MARS_WALLET_ADDRESS='${MARS_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export MARS_VALOPER='${HAQQ_VALOPER} >> $HOME/.bash_profile
source $HOME/.bash_profile
```
#
### `a few commands`
###
#### logs
```
sudo journalctl -u marsd -f --no-hostname -o cat
```

#### node sync status
```
marsd status 2>&1 | jq .SyncInfo
```

#### delegate 1 MARS (1000000 umars) to your validator
```
marsd tx staking delegate $MARS_VALOPER 1000000umars --from $MARS_WALLET -y
```

#### delegate 1 MARS (1000000 umars) to any validator
```
marsd tx staking delegate <validator operator address> 1000000umars --from $MARS_WALLET -y
```

#### collect rewards from all validators
```
marsd tx distribution withdraw-all-rewards --from $MARS_WALLET -y
```

#### collect rewards from a specific validator (remove <>)
```
marsd tx distribution withdraw-rewards <specific validator address> --commission --from $MARS_WALLET -y
```
#### check balance of your wallet
```
marsd q bank balances $MARS_WALLET_ADDRESS
```
  
#### check balance of a particular wallet (remove <>)
```
marsd q bank balances <particular wallet address>
```
