
## Lava. p2p RPC network. Install guide 
**lavad version v0.14.0**    
**upgrade block height: 254645**

#### *Links*
> - Website: https://lavanet.xyz/
> - Docs https://docs.lavanet.xyz/
> - Discord https://discord.gg/lavanetxyz
> - Explorer https://bd.lavanet.xyz/
> - Explorer NG https://lava.explorers.guru/

#### *Current version*
> Network Chain ID: `lava-testnet-1`    
> version: v0.14.0    
> commit: e7416e866f76f09a82b0f34030ca624c6e8f5396    

#### *Address books (first install node)*
> xalex    
> `wget -O $HOME/.lava/config/addrbook.json "https://raw.githubusercontent.com/toolfun/_nets/main/Lava/addrbook.json"`    
> lesnik    
> `wget -O $HOME/.lava/config/addrbook.json "https://share2.utsa.tech/lava/addrbook.json"`    
> nodejumper    
> `curl -s https://snapshots1-testnet.nodejumper.io/lava-testnet/addrbook.json > $HOME/.lava/config/addrbook.json`

#

### Update system and install tools
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl tar wget clang pkg-config libssl-dev build-essential bsdmainutils jq git make gcc ncdu chrony screen htop lz4 -y
```

### Install Go
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

### Set custom variables, e.g. `LAVA_NODENAME=JhonDoe`
```bash
LAVA_NODENAME=
LAVA_WALLET=
LAVA_PORT=
```
##### Set current chain ID 
```
LAVA_CHAIN_ID=lava-testnet-1
```
```
echo "export LAVA_NODENAME=$LAVA_NODENAME" >> $HOME/.bash_profile
echo "export LAVA_WALLET=$LAVA_WALLET" >> $HOME/.bash_profile
echo "export LAVA_CHAIN_ID=$LAVA_CHAIN_ID" >> $HOME/.bash_profile
echo "export LAVA_PORT=$LAVA_PORT" >> $HOME/.bash_profile
```
```
source ~/.bash_profile
```

### Install
```bash
cd $HOME
rm -rf lava
git clone https://github.com/lavanet/lava.git
cd lava
git checkout v0.14.0
make install
```
### Check version
```
lavad version --long | head | grep -e version: -e commit
```
> version: 0.14.0    
> commit: e7416e866f76f09a82b0f34030ca624c6e8f5396

### Configure
```
lavad config keyring-backend test
lavad config chain-id lava-testnet-1
lavad config node tcp://localhost:${LAVA_PORT}657
```

### Init
```
lavad init "$LAVA_NODENAME" --chain-id $LAVA_CHAIN_ID
```

### Set Lava-specific params

> create_empty_blocks = true    
> create_empty_blocks_interval = "60s"    
> timeout_propose = "60s"    
> timeout_commit = "60s"    
> timeout_broadcast_tx_commit = "601s"    

```
sed -i 's/create_empty_blocks = .*/create_empty_blocks = true/g' ~/.lava/config/config.toml
sed -i 's/create_empty_blocks_interval = ".*s"/create_empty_blocks_interval = "60s"/g' ~/.lava/config/config.toml
sed -i 's/timeout_propose = ".*s"/timeout_propose = "60s"/g' ~/.lava/config/config.toml
sed -i 's/timeout_commit = ".*s"/timeout_commit = "60s"/g' ~/.lava/config/config.toml
sed -i 's/timeout_broadcast_tx_commit = ".*s"/timeout_broadcast_tx_commit = "601s"/g' ~/.lava/config/config.toml
```

### Download genesis
```
curl -s https://raw.githubusercontent.com/K433QLtr6RA9ExEq/GHFkqmTzpdNLDd6T/main/testnet-1/genesis_json/genesis.json > $HOME/.lava/config/genesis.json
```

### Disable indexing (optional)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.lava/config/config.toml
```

### Ports for lavad
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${LAVA_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${LAVA_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${LAVA_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${LAVA_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${LAVA_PORT}660\"%" $HOME/.lava/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${LAVA_PORT}317\"%; s%^address = \":8080\"%address = \":${LAVA_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${LAVA_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${LAVA_PORT}091\"%" $HOME/.lava/config/app.toml
```

### Pruning
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="20"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.lava/config/app.toml
```

### Minimum gas price
```
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.025ulava"|g' $HOME/.lava/config/app.toml
```

### Reset chain
```
lavad tendermint unsafe-reset-all --home $HOME/.lava
```

### Create service
```
sudo tee /etc/systemd/system/lavad.service > /dev/null << EOF
[Unit]
Description=Lava Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which lavad) start
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

### Download one of address books. 
##### If node can't start syncing, download other one
> xalex addrbook
```
wget -O $HOME/.lava/config/addrbook.json "https://raw.githubusercontent.com/toolfun/_nets/main/Lava/addrbook.json"
```
> lesnik addrbook
```
wget -O $HOME/.lava/config/addrbook.json "https://share2.utsa.tech/lava/addrbook.json"
```
> nodejumper
```
curl -s https://snapshots1-testnet.nodejumper.io/lava-testnet/addrbook.json > $HOME/.lava/config/addrbook.json
```
> nodera
```
curl -s https://snapshots1-testnet.nodejumper.io/lava-testnet/addrbook.json > $HOME/.lava/config/addrbook.json
```

### Download snapshot and start. This part is correct even if already run a node. Thanks to [nodejumper](https://nodejumper.io/lava-testnet/sync)
```bash
# Service stop
sudo systemctl stop lavad

# Backup validator state file
cp $HOME/.lava/data/priv_validator_state.json $HOME/.lava/priv_validator_state.json.backup 

# Reset
lavad tendermint unsafe-reset-all --home $HOME/.lava --keep-addr-book 

# Download snapshot
curl https://snapshots1-testnet.nodejumper.io/lava-testnet/lava-testnet-1_2023-03-21.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.lava

# Restore validator state file
mv $HOME/.lava/priv_validator_state.json.backup $HOME/.lava/data/priv_validator_state.json 
```
```
sudo systemctl daemon-reload
sudo systemctl enable lavad
sudo systemctl start lavad
```

### Check logs
```
sudo journalctl -u lavad -f -o cat
```

### Add `OR` recover wallet
- Add    
```
lavad keys add $LAVA_WALLET
```
- Recover    
```
lavad keys add $LAVA_WALLET --recover
```

### Set variables with your wallet and valoper addresses
```
LAVA_WALLET_ADDRESS=$(haqqd keys show $LAVA_WALLET -a)
LAVA_VALOPER=$(haqqd keys show $LAVA_WALLET --bech val -a)
echo 'export LAVA_WALLET_ADDRESS='${LAVA_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export LAVA_VALOPER_ADDRESS='${LAVA_VALOPER} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Get tokens 

<!-- ##### Curl faucet ❗ Not available at the moment
> 1. Do
```
echo $(lavad keys show $LAVA_WALLET -a)
```
> 2. Copy and replace `WALLET ADRESS` with your copied address
```
curl -X POST -d '{"address": "WALLET ADDRESS", "coins": ["500000000ulava"]}' https://faucet-api.lavanet.xyz/faucet/
```
-->
> ##### Discord https://discord.gg/lavanetxyz
> ##### channel `#faucet` https://discord.com/channels/963778337904427018/1059851367717556314



### Query balance
```
lavad q bank balances $(lavad keys show $LAVA_WALLET -a)
```
> When you receive tokens, you can create validator

### Create validator
```
lavad tx staking create-validator \
--amount=80000ulava \
--pubkey=$(lavad tendermint show-validator) \
--moniker="LAVA_NODENAME" \
--chain-id=lava-testnet-1 \
--commission-rate=0.07 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.02 \
--min-self-delegation=1 \
--fees=5000ulava \
--from=$LAVA_WALLET\
-y
```

#

### Deleting
```
sudo systemctl disable --now lavad
sudo systemctl daemon-reload
cd $HOME
sudo rm -rf lava .lava $(which lavad) /etc/systemd/system/lavad.service
```

____
