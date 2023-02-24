### Haqq validator node install guide
- haqqd **v1.3.1**    
- chain ID: **haqq_54211-3**

#

### Update system and install some tools
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl tar wget clang pkg-config libssl-dev build-essential jq git make ncdu gcc chrony screen htop -y
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

### Set custom variables, e.g. `HAQQ_NODENAME=JhonDoe`
```bash
HAQQ_NODENAME=
HAQQ_WALLET=
HAQQ_PORT=
```
##### Set current chain ID 
```
HAQQ_CHAIN_ID=haqq_54211-3
```
```
echo "export HAQQ_NODENAME=$HAQQ_NODENAME" >> $HOME/.bash_profile
echo "export HAQQ_WALLET=$HAQQ_WALLET" >> $HOME/.bash_profile
echo "export HAQQ_CHAIN_ID=$HAQQ_CHAIN_ID" >> $HOME/.bash_profile
echo "export HAQQ_PORT=$HAQQ_PORT" >> $HOME/.bash_profile
```
```
source ~/.bash_profile
```

### Install
```
cd $HOME
git clone https://github.com/haqq-network/haqq
cd haqq
git checkout v1.3.1
make install 
```
> **Check version**    
```
haqqd version --long | grep -e "version:" -e "commit"
```
> version: '"v1.3.1"'    
> commit: 877c235c1b86b0c734fb482fdebdec71bdc47b07

### Configure
```
haqqd config chain-id $HAQQ_CHAIN_ID
haqqd config keyring-backend file
haqqd config node tcp://localhost:${HAQQ_PORT}657
```

### Initializing
```
haqqd init $HAQQ_NODENAME --chain-id $HAQQ_CHAIN_ID
```

### Genesis
```
wget -qO $HOME/.haqqd/config/genesis.json "https://raw.githubusercontent.com/haqq-network/validators-contest/master/genesis.json"
```

### Ports for haqqd
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${HAQQ_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${HAQQ_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${HAQQ_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${HAQQ_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${HAQQ_PORT}660\"%" $HOME/.haqqd/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${HAQQ_PORT}317\"%; s%^address = \":8080\"%address = \":${HAQQ_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${HAQQ_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${HAQQ_PORT}091\"%" $HOME/.haqqd/config/app.toml
```

### Pruning
Keep it default
<!--
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="20"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.haqqd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.haqqd/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.haqqd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.haqqd/config/app.toml
```
-->

### Indexer off
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.haqqd/config/config.toml
```

### Minimum gas price
```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0aISLM\"/" $HOME/.haqqd/config/app.toml
```

### Reset chain
```
haqqd tendermint unsafe-reset-all --home $HOME/.haqqd
```

### Create service for haqqd
```
sudo tee /etc/systemd/system/haqqd.service > /dev/null <<EOF
[Unit]
Description=Haqq_Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which haqqd) start --home $HOME/.haqqd
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Download and replace address book
```
rm $HOME/.haqqd/config/addrbook.json && wget -qO $HOME/.haqqd/config/addrbook.json "https://raw.githubusercontent.com/toolfun/_nets/main/Haqq/addrbook.json"
```

### Choose one option how to start. Start with *state sync* `OR` *snapshot*. We will use [kjnodes](https://services.kjnodes.com/home/testnet/haqq/state-sync) service
> 1. State sync
```bash
# Prepare
sudo systemctl stop haqqd
cp $HOME/.haqqd/data/priv_validator_state.json $HOME/.haqqd/priv_validator_state.json.backup
haqqd tendermint unsafe-reset-all --home $HOME/.haqqd
```
```bash
# State sync options
STATE_SYNC_RPC=https://haqq-testnet.rpc.kjnodes.com:443
STATE_SYNC_PEER=d5519e378247dfb61dfe90652d1fe3e2b3005a5b@haqq-testnet.rpc.kjnodes.com:35656
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 2000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -e "s|^enable *=.*|enable = true|" $HOME/.haqqd/config/config.toml
sed -i.bak -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  $HOME/.haqqd/config/config.toml
sed -i.bak -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  $HOME/.haqqd/config/config.toml
sed -i.bak -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  $HOME/.haqqd/config/config.toml
sed -i.bak -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.haqqd/config/config.toml
mv $HOME/.haqqd/priv_validator_state.json.backup $HOME/.haqqd/data/priv_validator_state.json
```
```bash 
# Enable service and (re)start
sudo systemctl daemon-reload
sudo systemctl enable haqqd
sudo systemctl restart haqqd && sudo journalctl -u haqqd -f -o cat
```

> 2. Snapshot    
```bash
# Prepare
sudo systemctl stop haqqd
cp $HOME/.haqqd/data/priv_validator_state.json $HOME/.haqqd/priv_validator_state.json.backup
rm -rf $HOME/.haqqd/data
```
```bash
# Download snapshot
curl -L https://snapshots.kjnodes.com/haqq-testnet/snapshot_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.haqqd
mv $HOME/.haqqd/priv_validator_state.json.backup $HOME/.haqqd/data/priv_validator_state.json
```
```bash
# Enable service and (re)start
sudo systemctl daemon-reload
sudo systemctl enable haqqd
sudo systemctl restart haqqd && sudo journalctl -u haqqd -f -o cat
```

### Add `OR` recover wallet
- Add    
```
haqqd keys add $HAQQ_WALLET
```
- Recover    
```
haqqd keys add $HAQQ_WALLET --recover
```

### Get tokens for creating validator
##### 1. You need private key of the wallet
```
haqqd keys export $HAQQ_WALLET --unarmored-hex --unsafe
```
##### 2. Import private key to the Metamask
##### 3. Connect your Metamask to the faucet https://testedge2.haqq.network/
##### 4. Login with your github account 
##### 5. Claim tokens

### Create validator
```
haqqd tx staking create-validator \
--amount 1000000000000000000aISLM \
--pubkey $(haqqd tendermint show-validator) \
--commission-rate 0.07 \
--commission-max-rate 0.25 \
--commission-max-change-rate 0.05 \
--min-self-delegation "1000000" \
--moniker $HAQQ_NODENAME \
--from $HAQQ_WALLET \
--gas=auto \
--fees 500aISLM
```

### Set variables with your wallet and valoper addresses
```
HAQQ_WALLET_ADDRESS=$(haqqd keys show $HAQQ_WALLET -a)
HAQQ_VALOPER=$(haqqd keys show $HAQQ_WALLET --bech val -a)
echo 'export HAQQ_WALLET_ADDRESS='${HAQQ_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export HAQQ_VALOPER='${HAQQ_VALOPER} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

____
