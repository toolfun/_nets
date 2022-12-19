## 🚧🚧 under construction 

### Server prepare
#### Updating
```
sudo apt update && upt upgrade
```
#### Instal packages
```
sudo apt install curl wget git jq lz4 build-essential unzip tmux htop ncdu snapd -y
```

### Set variables, replace with your values
```
export HUMANS_NODENAME=YOUR's
export HUMANS_PORT=18
export HUMANS_WALLET=YOUR's
export HUMANS_CHAIN_ID=testnet-1
```

#### Install (compile the source code)
```
rm -rf humans
cd $HOME
git clone https://github.com/humansdotai/humans
cd humans
git checkout v1.0.0
go build -o humansd cmd/humansd/main.go
```
```bash
# After build, copy directly to a /usr/local/bin folder
sudo cp humansd /usr/local/bin/humansd
```
```
humansd config keyring-backend test
humansd config chain-id $HUMANS_CHAIN_ID
humansd init $HUMANS_NODENAME --chain-id $HUMANS_CHAIN_ID
```

#
____

### Download genesis
```
curl -s https://rpc-testnet.humans.zone/genesis | jq -r .result.genesis > $HOME/.humans/config/genesis.json
sha256sum $HOME/.humans/config/genesis.json # f5fef1b574a07965c005b3d7ad013b27db197f57146a12c018338d7e58a4b5cd
```
### Set gas-prices
```
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.025uheart"|g' $HOME/.humans/config/app.toml
```

### Set seeds and/or peers
```
seeds=""
peers="e4234a5fba85b2c3d2ad157b6961ac3d115f4c49@humans-testnet.nodejumper.io:28656,1df6735ac39c8f07ae5db31923a0d38ec6d1372b@45.136.40.6:26656,9726b7ba17ee87006055a9b7a45293bfd7b7f0fc@45.136.40.16:26656,6e84cde074d4af8a9df59d125db3bf8d6722a787@45.136.40.18:26656,eda3e2255f3c88f97673d61d6f37b243de34e9d9@45.136.40.13:26656,4de8c8acccecc8e0bed4a218c2ef235ab68b5cf2@45.136.40.12:26656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.humans/config/config.toml
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

### Configure pruning
```
sed -i 's|pruning = "default"|pruning = "custom"|g' $HOME/.humans/config/app.toml
sed -i 's|pruning-keep-recent = "0"|pruning-keep-recent = "100"|g' $HOME/.humans/config/app.toml
sed -i 's|pruning-interval = "0"|pruning-interval = "17"|g' $HOME/.humans/config/app.toml
```

### Create service for node
```
sudo tee /etc/systemd/system/humansd.service > /dev/null << EOF
[Unit]
Description=Humans.ai node
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

### Download snapshot
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
```
sudo journalctl -u humansd -f --no-hostname -o cat
```


