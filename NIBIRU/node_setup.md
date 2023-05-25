## Nibiru Incentivized Testnet (NIT). 4 phases, March - June 2023
NIT1 - Oracles and Staking    
NIT2 - Governance    
NIT3 - Liquidity Applications Perps & Spot    
NIT4 - Trading competition

____

> ### 🧰 helps    
>- 📖 addressbook:    
>`wget -O $HOME/.nibid/config/addrbook.json https://raw.githubusercontent.com/toolfun/_nets/main/NIBIRU/addrbook.json`    
>- 📖 OR addressbook:    
>`curl -Ls https://snapshots.kjnodes.com/nibiru-testnet/addrbook.json > $HOME/.nibid/config/addrbook.json`    
>- 💧  faucet:    
`FAUCET_URL="https://faucet.testnet-2.nibiru.fi/"`    
`ADDRESS=""` *(put your address in "")*    
`curl -X POST -d '{"address": "'"$ADDRESS"'", "coins": ["110000000unibi","100000000unusd","100000000uusdt"]}' $FAUCET_URL`    
>
>      Discord faucet https://discord.com/channels/947911971515293759/984840062871175219    
>- [Restart from snapshot](https://github.com/toolfun/nets/blob/main/NIBIRU/nibiru-incentivized-testnet.md#useful-stuff)
>- [Delete the node](https://github.com/toolfun/_nets/blob/main/NIBIRU/node_setup.md#delete-nibiru)

____

### Links
- [Website](https://nibiru.fi/)
- [Twitter](https://twitter.com/NibiruChain)
- [Discord](https://discord.gg/y2gf9xRM) (invite link)
- [Discord](https://discord.gg/nibirufi)
- [Blog](https://blog.nibiru.fi/)
- [Docs](https://docs.nibiru.fi/)
- [Explorer1](https://exp.utsa.tech/nibiru-test)
- [Explorer2](https://nibiru.explorers.guru/)

### Minimum hardware requirements
- 4CPU
- 16GB RAM
- 500GB of disk space (SSD)
____

### Set custom variables

Set the wallet name, e.g. *NIBIRU_WALLET=mywallet*    
Set the node name, e.g. *NIBIRU_NODENAME=JhonDoe*, it will be dispalayed as your validator name (can be changed)    
Set port, e.g. 17    
```
NIBIRU_NODENAME=
NIBIRU_WALLET=
NIBIRU_PORT=17
```

### Set Nibiru chain ID
```
NIBIRU_CHAIN_ID=nibiru-itn-1
```

```
echo 'export NIBIRU_NODENAME='\"${NIBIRU_NODENAME}\" >> $HOME/.bash_profile
echo 'export NIBIRU_PORT='\"${NIBIRU_PORT}\" >> $HOME/.bash_profile
echo 'export NIBIRU_WALLET='\"${NIBIRU_WALLET}\" >> $HOME/.bash_profile
echo 'export NIBIRU_CHAIN_ID='\"${NIBIRU_CHAIN_ID}\" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

# Update and upgrade
```
sudo apt update
sudo apt install make gcc clang pkg-config libssl-dev build-essential git jq ncdu bsdmainutils htop lz4 -y < "/dev/null"
```

### Install Go
```
cd $HOME
wget -O go1.20.3.linux-amd64.tar.gz https://golang.org/dl/go1.20.3.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz && rm go1.20.3.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
```
#### Check
```
go version
```

### Download and install nibiru
```bash
cd $HOME
git clone https://github.com/NibiruChain/nibiru
cd nibiru
git checkout v0.19.2
make install
```

### Init
```
nibid init $NIBIRU_NODENAME --chain-id=$NIBIRU_CHAIN_ID
```

### Download genesis
```
curl -s https://rpc.itn-1.nibiru.fi/genesis | jq -r .result.genesis > $HOME/.nibid/config/genesis.json

```

### Config client
```
nibid config chain-id $NIBIRU_CHAIN_ID
nibid config keyring-backend test
nibid config node tcp://localhost:${NIBIRU_PORT}657
```

### Set custom ports
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${NIBIRU_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${NIBIRU_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${NIBIRU_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${NIBIRU_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${NIBIRU_PORT}660\"%" $HOME/.nibid/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${NIBIRU_PORT}317\"%; s%^address = \":8080\"%address = \":${NIBIRU_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${NIBIRU_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${NIBIRU_PORT}091\"%" $HOME/.nibid/config/app.toml
```
### Set min gas price
```bash
sed -i.default 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025unibi"/g' $HOME/.nibid/config/app.toml
```

### Pruning (optional)
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.nibid/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.nibid/config/app.toml
```

<!-- ##########################################
new version v0.16.2 include these in binary itself```
CONFIG_TOML="$HOME/.nibid/config/config.toml"
sed -i 's/timeout_propose =.*/timeout_propose = "100ms"/g' $CONFIG_TOML
sed -i 's/timeout_propose_delta =.*/timeout_propose_delta = "500ms"/g' $CONFIG_TOML
sed -i 's/timeout_prevote =.*/timeout_prevote = "100ms"/g' $CONFIG_TOML
sed -i 's/timeout_prevote_delta =.*/timeout_prevote_delta = "500ms"/g' $CONFIG_TOML
sed -i 's/timeout_precommit =.*/timeout_precommit = "100ms"/g' $CONFIG_TOML
sed -i 's/timeout_precommit_delta =.*/timeout_precommit_delta = "500ms"/g' $CONFIG_TOML
sed -i 's/timeout_commit =.*/timeout_commit = "1s"/g' $CONFIG_TOML
sed -i 's/skip_timeout_commit =.*/skip_timeout_commit = false/g' $CONFIG_TOML
sed -i.default "s/pruning *=.*/pruning = \"custom\"/g" $HOME/.nibid/config/app.toml
sed -i "s/pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/g" $HOME/.nibid/config/app.toml
sed -i "s/pruning-interval *=.*/pruning-interval = \"10\"/g" $HOME/.nibid/config/app.toml
```
###########################################-->

### Turn off indexer (optional)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.nibid/config/config.toml
rm -rf $HOME/.nibid/data/tx_index.db
```

### Add new wallet or exsisting one
```bash
# New
nibid keys add $NIBIRU_WALLET
# Exsisting
nibid keys add $NIBIRU_WALLET --recover
```

### Add wallet and valoper addresses as variables
```
NIBIRU_WALLET_ADDRESS=$(nibid keys show $NIBIRU_WALLET -a)
```
```
NIBIRU_VALOPER=$(nibid keys show $NIBIRU_WALLET --bech val -a)
```
```
echo 'export NIBIRU_WALLET_ADDRESS='\"${NIBIRU_WALLET_ADDRESS}\" >> $HOME/.bash_profile
echo 'export NIBIRU_VALOPER='\"${NIBIRU_VALOPER}\" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Service file
```
sudo tee /etc/systemd/system/nibid.service > /dev/null <<EOF
[Unit]
Description=Nibiru node
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

### ~~State sync~~
#### Reset
```
nibid tendermint unsafe-reset-all --home $HOME/.nibid
```
### Download addressbook
```
curl -Ls https://snapshots.kjnodes.com/nibiru-testnet/addrbook.json > $HOME/.nibid/config/addrbook.json
```

### Download snapshot (thanks to kjnodes)
```
curl -L https://snapshots.kjnodes.com/nibiru-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.nibid
```

### Start the node
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable nibid
sudo systemctl restart nibid
```

#### Check logs
```
journalctl -u nibid -f -o cat
```
  
### Faucet nibi140qwnd0n4qtlqkgjne6v7ddte9h7s08f6an36z
```
curl -X POST -d '{"address": "'"nibi140qwnd0n4qtlqkgjne6v7ddte9h7s08f6an36z"'", "coins": ["10000000unibi","100000000000unusd"]}' https://faucet.testnet-2.nibiru.fi/
```

### Create validator
```
nibid tx staking create-validator \
--amount=1000000unibi \
--pubkey=$(nibid tendermint show-validator) \
--moniker="$NIBIRU_NODENAME" \
--chain-id=$NIBIRU_CHAIN_ID \
--commission-rate="0.07" \
--commission-max-rate="0.20" \
--commission-max-change-rate="0.02" \
--min-self-delegation="1000000" \
--from=$NIBIRU_WALLET \
--fees=5000unibi \
-y
```

____

## Useful stuff
### Restart from snapshot (by kjnodes)
#### Install lz4
```
sudo apt update && sudo apt install lz4 -y
```

#### Disable state sync
```
sed -i 's/enable = true/enable = false/g' $HOME/.nibid/config/config.toml
```

#### Stop nibiru and clear old data
```
sudo systemctl stop nibid
cp $HOME/.nibid/data/priv_validator_state.json $HOME/.nibid/priv_validator_state.json.backup
rm -rf $HOME/.nibid/data
```

#### Download snapshot
```
curl -L https://snapshots.kjnodes.com/nibiru-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.nibid
mv $HOME/.nibid/priv_validator_state.json.backup $HOME/.nibid/data/priv_validator_state.json
```

#### Restart nibiru and check
```
sudo systemctl start nibid && journalctl -u nibid -f -o cat
```

#

### Delete nibiru
📌 Make sure you have made backups:
1. **wallet mnemonic**    
2. **private validator key** which is `~/.nibid/config/priv_validator_key.json`    

```
cd $HOME
systemctl disable --now nibid
systemctl daemon-reload && \
rm -rf /etc/systemd/system/nibid.service .nibid nibiru $(which nibid)
```
