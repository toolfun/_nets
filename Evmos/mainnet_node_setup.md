`# mainnet`
`# evmosd v15.0.0`

### Installing Evmos node for the Lava Evmos ipRPC
##### https://www.lavanet.xyz/blog/providers-guide-to-evmos-iprpc
> This manual is intended for installing an Evmos mainnet node on the same server as testnet, without using containers or else. Therefore, the `--home $HOME/.evmosdt` option is added for most binary commands

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
cd
rm -r $HOME/evmos
git clone https://github.com/evmos/evmos
cd evmos
git checkout v15.0.0
make install
```

### Customizing with variables (moniker will be visible in blockchain, wallet name is not)
```
moniker=
wallet=
chain=evmos_9001-2
```

You can customize which ports will be used, or do nothing and leave as default.
My custom port is
```
port=12
```

```
echo "export EVMOS_M=$moniker" >> $HOME/.bash_profile
echo "export EVMOS_W=$wallet" >> $HOME/.bash_profile
echo "export EVMOS_CHAIN=$chain" >> $HOME/.bash_profile
echo "export EVMOS_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Init with a custom dir `evmosd`
```
evmosd init $EVMOS_M --chain-id $EVMOS_CHAIN --home $HOME/.evmosd
```

### Genesis download
```
wget -O $HOME/.evmosd/config/genesis.json https://archive.evmos.org/mainnet/genesis.json
```

### Addressbook
```
wget -O $HOME/.evmosd/config/addrbook.json https://ss.evmos.nodestake.top/addrbook.json
```

### App config
```
evmosd config chain-id $EVMOS_CHAIN --home $HOME/.evmosd
evmosd config node tcp://localhost:${EVMOS_PORT}657 --home $HOME/.evmosd
evmosd config keyring-backend file --home $HOME/.evmosd
```

### Minimum gas prices
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.00000002aevmos\"/;" ~/.evmosd/config/app.toml
```

### Pruning settings
```
pruning="custom"
pruning_keep_recent="50000"
pruning_interval="1000"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.evmosd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.evmosd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.evmosd/config/app.toml
```

### Ports customizing
```
sed -i.bak \
-e "s%^proxy_app = \"tcp://127.0.0.1:[0-9]\{2\}658\"%proxy_app = \"tcp://127.0.0.1:${EVMOS_PORT}658\"%;" \
-e "s%^laddr = \"tcp://127.0.0.1:[0-9]\{2\}657\"%laddr = \"tcp://127.0.0.1:${EVMOS_PORT}657\"%;" \
-e "s%^pprof_laddr = \"localhost:[0-9]\{1\}060\"%pprof_laddr = \"localhost:${EVMOS_PORT}060\"%;" \
-e "s%^laddr = \"tcp://0.0.0.0:[0-9]\{2\}656\"%laddr = \"tcp://0.0.0.0:${EVMOS_PORT}656\"%;" \
-e "s%^prometheus_listen_addr = \":[0-9]\{2\}660\"%prometheus_listen_addr = \":${EVMOS_PORT}660\"%" \
$HOME/.evmosd/config/config.toml

sed -i.bak \
-e "s%^address = \"tcp://localhost:[0-9]\{1\}317\"%address = \"tcp://localhost:${EVMOS_PORT}317\"%;" \
-e "s%^address = \":[0-9]\{1\}080\"%address = \":${EVMOS_PORT}080\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}090\"%address = \"localhost:${EVMOS_PORT}090\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}091\"%address = \"localhost:${EVMOS_PORT}091\"%;" \
-e "s%^address = \"127.0.0.1:[0-9]\{2\}45\"%address = \"127.0.0.1:${EVMOS_PORT}45\"%;" \
-e "s%^ws-address = \"127.0.0.1:[0-9]\{2\}46\"%ws-address = \"127.0.0.1:${EVMOS_PORT}46\"%;" \
-e "s%^metrics-address = \"127.0.0.1:[0-9]\{2\}65\"%metrics-address = \"127.0.0.1:${EVMOS_PORT}65\"%" \
$HOME/.evmosd/config/app.toml
```

### Service file
```
sudo tee /etc/systemd/system/evmosd.service > /dev/null <<EOF
[Unit]
Description=Evmos_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which evmosd) start --home $HOME/.evmosd
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Reset state
```
evmosd tendermint unsafe-reset-all --home $HOME/.evmosd
```

### Snapshot or state-sync start
https://polkachu.com/networks/evmos

### Service start
```
sudo systemctl daemon-reload
sudo systemctl enable --now evmosd && journalctl -u evmosd -f -o cat
```

____

## Useful

### With 2 evmos nodes on a one server use `--home` option
For example
```javascript
evmosd status --home $HOME/.evmosd | jq .SyncInfo
```
