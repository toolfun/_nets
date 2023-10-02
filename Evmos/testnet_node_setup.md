### 🚧🚧🚧 under construction
#
### installing evmos testnet node for the Lava Evmos RPC
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
```
moniker=xAlex
wallet=wAlex
chain=evmos_9000-4
```

You can customize which ports will be used, or do nothing and leave as default.
My custom port is
```
port=12
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
curl -Ls https://snapshots.kjnodes.com/evmos-testnet/addrbook.json > $HOME/.evmosd/config/addrbook.json

```

### App config
```
evmosdt config chain-id $EVMOST_CHAIN --home $HOME/.evmosdt
evmosdt config node tcp://localhost:${EVMOST_PORT}657
evmosdt config keyring-backend test
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
-e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${EVMOST_PORT}658\"%;" \
-e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${EVMOST_PORT}657\"%;" \
-e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${EVMOST_PORT}060\"%;" \
-e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${EVMOST_PORT}656\"%;" \
-e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${EVMOST_PORT}660\"%" \
$HOME/.evmosdt/config/config.toml

sed -i.bak \
-e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:${EVMOST_PORT}317\"%;" \
-e "s%^address = \":8080\"%address = \":${EVMOST_PORT}080\"%;" \
-e "s%^address = \"localhost:9090\"%address = \"localhost:${EVMOST_PORT}090\"%;" \
-e "s%^address = \"localhost:9091\"%address = \"localhost:${EVMOST_PORT}091\"%;" \
-e "s%^address = \"127.0.0.1:8545\"%address = \"127.0.0.1:${EVMOST_PORT}45\"%;" \
-e "s%^ws-address = \"127.0.0.1:8546\"%ws-address = \"127.0.0.1:${EVMOST_PORT}46\"%;" \
-e "s%^metrics-address = \"127.0.0.1:6065\"%metrics-address = \"127.0.0.1:${EVMOST_PORT}65\"%" \
$HOME/.evmosdt/config/app.toml
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

###
```

```

###
```

```

###
```

```
