### 🚧🚧🚧 under construction

____

<!-- ###########################################################
###
### Update system and install tools
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl tar wget clang pkg-config libssl-dev build-essential bsdmainutils jq git make gcc lz4 -y
```

### go
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

### install
```
cd $HOME
rm -rf lava
git clone https://github.com/lavanet/lava.git
cd lava
git checkout v0.24.0
# The binary for a provider
export LAVA_BINARY=lavap
make install
```

### variables
```
node_moniker=
wallet=
provider_wallet=
chain=lava-testnet-2
port=13
```

### write and load
```
echo "export LAVA_M=$node_moniker" >> $HOME/.bash_profile
echo "export LAVA_W=$wallet" >> $HOME/.bash_profile
echo "export LAVARPC_W=$provider_wallet" >> $HOME/.bash_profile
echo "export LAVA_CHAIN=$chain" >> $HOME/.bash_profile
echo "export LAVA_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### config
```
lavad config keyring-backend test
lavad config chain-id lava-testnet-2
lavad config node tcp://localhost:${LAVA_PORT}657
```

### init
```
lavad init $LAVA_M --chain-id $LAVA_CHAIN
```

### config timeouts
```
sed -i \
  -e 's/timeout_commit = ".*"/timeout_commit = "30s"/g' \
  -e 's/timeout_propose = ".*"/timeout_propose = "1s"/g' \
  -e 's/timeout_precommit = ".*"/timeout_precommit = "1s"/g' \
  -e 's/timeout_precommit_delta = ".*"/timeout_precommit_delta = "500ms"/g' \
  -e 's/timeout_prevote = ".*"/timeout_prevote = "1s"/g' \
  -e 's/timeout_prevote_delta = ".*"/timeout_prevote_delta = "500ms"/g' \
  -e 's/timeout_propose_delta = ".*"/timeout_propose_delta = "500ms"/g' \
  -e 's/skip_timeout_commit = ".*"/skip_timeout_commit = false/g' \
  $HOME/.lava/config/config.toml
```

### config ports
```
sed -i.bak \
-e "s%^proxy_app = \"tcp://127.0.0.1:[0-9]\{2\}658\"%proxy_app = \"tcp://127.0.0.1:${LAVA_PORT}658\"%;" \
-e "s%^laddr = \"tcp://127.0.0.1:[0-9]\{2\}657\"%laddr = \"tcp://127.0.0.1:${LAVA_PORT}657\"%;" \
-e "s%^pprof_laddr = \"localhost:[0-9]\{1\}060\"%pprof_laddr = \"localhost:${LAVA_PORT}060\"%;" \
-e "s%^laddr = \"tcp://0.0.0.0:[0-9]\{2\}656\"%laddr = \"tcp://0.0.0.0:${LAVA_PORT}656\"%;" \
-e "s%^prometheus_listen_addr = \":[0-9]\{2\}660\"%prometheus_listen_addr = \":${LAVA_PORT}660\"%" \
$HOME/.lava/config/config.toml
sed -i.bak \
-e "s%^address = \"tcp://localhost:[0-9]\{1\}317\"%address = \"tcp://localhost:${LAVA_PORT}317\"%;" \
-e "s%^address = \":[0-9]\{1\}080\"%address = \":${LAVA_PORT}080\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}090\"%address = \"localhost:${LAVA_PORT}090\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}091\"%address = \"localhost:${LAVA_PORT}091\"%;" \
$HOME/.lava/config/app.toml
```

### minimum gas price
```
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0ulava"|g' $HOME/.lava/config/app.toml
```

### Pruning
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.lava/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.lava/config/app.toml
```

### disable indexing (optional)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.lava/config/config.toml
```

### reset
```
lavad tendermint unsafe-reset-all --home $HOME/.lava
```

### node service
```
sudo tee /etc/systemd/system/lavad.service > /dev/null << EOF
[Unit]
Description=Lava_node
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

###
```
curl -Ls https://snapshots.kjnodes.com/lava-testnet/addrbook.json > $HOME/.lava/config/addrbook.json

```

### [kjnode's](https://services.kjnodes.com/) snapshot
```
sudo systemctl start lavad && sudo journalctl -u lavad -f --no-hostname -o cat
```

### add wallet
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

######################################################### -->
