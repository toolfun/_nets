`# testnet`
### Installing Evmos testnet node for the Lava Evmos ipRPC
##### https://www.lavanet.xyz/blog/providers-guide-to-evmos-iprpc
> This manual is intended for installing an Evmos testnet node on the same server as mainnet, without using containers. Therefore, the `--home $HOME/.evmosdt` option is added for most binary commands

____

### System update
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### Go installation
```
if ! [ -x "$(command -v go)" ]; then
  ver="1.20.5"
  cd $HOME
  wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
  sudo rm "go$ver.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
fi
```

### Binary installation
```
git clone https://github.com/evmos/evmos
cd evmos
git checkout v14.0.0-rc5
make install
```

### Rename binary for the convenience
```
mv ~/go/bin/evmosd ~/go/bin/evmosdt
```

### Customizing with variables
```bash
# choose displayed name aka validator name
moniker=
# choose wallet name
wallet=
# chain-id leave as is
chain=evmos_9000-4
```

You can customize which ports will be used, or do nothing and leave as default.
My custom port is
```
port=11
```

```
echo "export EVMOST_M=$moniker" >> $HOME/.bash_profile
echo "export EVMOST_W=$wallet" >> $HOME/.bash_profile
echo "export EVMOST_CHAIN=$chain" >> $HOME/.bash_profile
echo "export EVMOST_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Init with a custom dir `evmosdt`
```
evmosdt init $EVMOST_M --chain-id $EVMOST_CHAIN --home $HOME/.evmosdt
```

### Genesis download
```
wget -O $HOME/.evmosdt/config/genesis.json "https://ss-t.evmos.nodestake.top/genesis.json"
```

### Addressbook
```
curl -Ls https://snapshots.kjnodes.com/evmos-testnet/addrbook.json > $HOME/.evmosdt/config/addrbook.json

```

### Reset state
```
evmosdt tendermint unsafe-reset-all --home $HOME/.evmosdt
```

### App config
```
evmosdt config chain-id $EVMOST_CHAIN --home $HOME/.evmosdt
evmosdt config node tcp://localhost:${EVMOST_PORT}657 --home $HOME/.evmosdt
evmosdt config keyring-backend test --home $HOME/.evmosdt
```

### Minimum gas prices
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0001aevmos\"/;" ~/.evmosdt/config/app.toml
```

### Pruning settings
```
pruning="custom"
pruning_keep_recent="50000"
pruning_interval="1000"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.evmosdt/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.evmosdt/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.evmosdt/config/app.toml
```

### Ports customizing
```
sed -i.bak \
sed -i.bak \
-e "s%^proxy_app = \"tcp://127.0.0.1:[0-9]\{2\}658\"%proxy_app = \"tcp://127.0.0.1:${EVMOS_PORT}658\"%;" \
-e "s%^laddr = \"tcp://127.0.0.1:[0-9]\{2\}657\"%laddr = \"tcp://127.0.0.1:${EVMOS_PORT}657\"%;" \
-e "s%^pprof_laddr = \"localhost:[0-9]\{1\}060\"%pprof_laddr = \"localhost:${EVMOS_PORT}060\"%;" \
-e "s%^laddr = \"tcp://0.0.0.0:[0-9]\{2\}656\"%laddr = \"tcp://0.0.0.0:${EVMOS_PORT}656\"%;" \
-e "s%^prometheus_listen_addr = \":[0-9]\{2\}660\"%prometheus_listen_addr = \":${EVMOS_PORT}660\"%" \
$HOME/.evmosdt/config/config.toml

sed -i.bak \
-e "s%^address = \"tcp://localhost:[0-9]\{1\}317\"%address = \"tcp://localhost:${EVMOS_PORT}317\"%;" \
-e "s%^address = \":[0-9]\{1\}080\"%address = \":${EVMOS_PORT}080\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}090\"%address = \"localhost:${EVMOS_PORT}090\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}091\"%address = \"localhost:${EVMOS_PORT}091\"%;" \
-e "s%^address = \"127.0.0.1:[0-9]\{2\}45\"%address = \"127.0.0.1:${EVMOS_PORT}45\"%;" \
-e "s%^ws-address = \"127.0.0.1:[0-9]\{2\}46\"%ws-address = \"127.0.0.1:${EVMOS_PORT}46\"%;" \
-e "s%^metrics-address = \"127.0.0.1:[0-9]\{2\}65\"%metrics-address = \"127.0.0.1:${EVMOS_PORT}65\"%" \
$HOME/.evmosdt/config/app.toml
```

### Service file
```
sudo tee /etc/systemd/system/evmosdt.service > /dev/null <<EOF
[Unit]
Description=Evmos_TESTNET_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which evmosdt) start --home $HOME/.evmosdt
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Snapshot or state-sync start
state-sync via [kjnode's](https://services.kjnodes.com/)
```
STATE_SYNC_RPC=https://evmos-testnet.rpc.kjnodes.com:443
STATE_SYNC_PEER=d5519e378247dfb61dfe90652d1fe3e2b3005a5b@evmos-testnet.rpc.kjnodes.com:16756
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 1000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i \
  -e "s|^enable *=.*|enable = true|" \
  -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.evmosdt/config/config.toml
```

### Service start
```
sudo systemctl daemon-reload
sudo systemctl enable --now evmosdt && journalctl -u evmosdt -f -o cat
```

____

## Useful

### With 2 evmos nodes on a one server use `--home` option
For example
```python
evmosdt status --home $HOME/.evmosdt | jq .SyncInfo
```
