### Humans.ai
#### *A human behind every AI decision*
### Validator install guide

#

> - site: https://humans.ai/proof-of-human
> - explorer: https://explorer.humans.zone/humans-testnet
> 
> Hardware spec, minimum: 4CPU / 8GB RAM / 100GB SSD

____

### Update and upgrade
```
sudo apt update && upt upgrade -y
```

### Instal tools
```
sudo apt install curl tar wget clang pkg-config libssl-dev build-essential bsdmainutils jq git make ncdu gcc chrony screen htop -y
```

### Set custom variables
- #### e.g. `HUMANS_NODENAME=JhonDoe`

```
HUMANS_NODENAME=
HUMANS_PORT=
HUMANS_WALLET=
```
- #### Chain
```
HUMANS_CHAIN_ID=testnet-1
```
- #### Environment
```
echo "export HUMANS_NODENAME=$HUMANS_NODENAME" >> $HOME/.bash_profile
echo "export HUMANS_WALLET=$HUMANS_WALLET" >> $HOME/.bash_profile
echo "export HUMANS_CHAIN_ID=$HUMANS_CHAIN_ID" >> $HOME/.bash_profile
echo "export HUMANS_PORT=$HUMANS_PORT" >> $HOME/.bash_profile
```
- #### Load
```
source ~/.bash_profile
```

### Install go
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

### Build binary
```
rm -rf humans
cd $HOME
git clone https://github.com/humansdotai/humans
cd humans
git checkout v1.0.0
go build -o humansd cmd/humansd/main.go
```
```
mv humansd $HOME/go/bin/humansd
```

### Config app
```
humansd config keyring-backend test
```

### Init
```
humansd init $HUMANS_NODENAME --chain-id $HUMANS_CHAIN_ID
```

### Download genesis
```
curl -s https://rpc-testnet.humans.zone/genesis | jq -r .result.genesis > $HOME/.humans/config/genesis.json
sha256sum $HOME/.humans/config/genesis.json # f5fef1b574a07965c005b3d7ad013b27db197f57146a12c018338d7e58a4b5cd
```
### Set gas-prices
```
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.025uheart"|g' $HOME/.humans/config/app.toml
```

### ~Set seeds and/or peers~

### Download address book
```
wget -O $HOME/.humans/config/addrbook.json https://raw.githubusercontent.com/toolfun/_nets/main/Humans.ai/addrbook.json
```

### Set custom timeouts
```
sed -i 's|^timeout_propose =.*$|timeout_propose = "100ms"|' $HOME/.humans/config/config.toml
sed -i 's|^timeout_propose_delta =.*$|timeout_propose_delta = "500ms"|' $HOME/.humans/config/config.toml
sed -i 's|^timeout_prevote =.*$|timeout_prevote = "100ms"|' $HOME/.humans/config/config.toml
sed -i 's|^timeout_prevote_delta =.*$|timeout_prevote_delta = "500ms"|' $HOME/.humans/config/config.toml
sed -i 's|^timeout_precommit =.*$|timeout_precommit = "100ms"|' $HOME/.humans/config/config.toml
sed -i 's|^timeout_precommit_delta =.*$|timeout_precommit_delta = "500ms"|' $HOME/.humans/config/config.toml
sed -i 's|^timeout_commit =.*$|timeout_commit = "1s"|' $HOME/.humans/config/config.toml
sed -i 's|^skip_timeout_commit =.*$|skip_timeout_commit = false|' $HOME/.humans/config/config.toml
```

### Configure pruning (not mandatory)
```
sed -i 's|pruning = "default"|pruning = "custom"|g' $HOME/.humans/config/app.toml
sed -i 's|pruning-keep-recent = "0"|pruning-keep-recent = "100"|g' $HOME/.humans/config/app.toml
sed -i 's|pruning-interval = "0"|pruning-interval = "10"|g' $HOME/.humans/config/app.toml
```

### Disable indexing (not mandatory)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.humans/config/config.toml
```

### Create service for a node
```
sudo tee /etc/systemd/system/humansd.service > /dev/null << EOF
[Unit]
Description=Humans.ai Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which humansd) start
Restart=on-failure
RestartSec=5
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target
EOF
```

### Reset chain
```
humansd tendermint unsafe-reset-all --home $HOME/.humans --keep-addr-book
```

### Download snapshot (nodejumper)
```
SNAP_NAME=$(curl -s https://snapshots4-testnet.nodejumper.io/humans-testnet/ | egrep -o ">testnet-1.*\.tar.lz4" | tr -d ">")
curl https://snapshots4-testnet.nodejumper.io/humans-testnet/${SNAP_NAME} | lz4 -dc - | tar -xf -
```

#### Restart from snapshot
```
sudo systemctl daemon-reload
sudo systemctl enable humansd
sudo systemctl restart humansd
```

### Logs
```
sudo journalctl -u humansd -f --no-hostname -o cat
```

#

### All set!


