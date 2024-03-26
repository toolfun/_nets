`side-testnet-3` `v0.7.0`
# Side

### System update
```

```

### Go installation
```

```

### Binary installation
```
cd
sudo rm -r side
git clone https://github.com/sideprotocol/side.git
cd side
git checkout v0.7.0
make install
```

### Variables
```
moniker=xAlex
wallet=wAlex
chain=side-testnet-3
port=19
```

### Save and load variables
```
echo "export AXELAR_M=$moniker" >> $HOME/.bash_profile
echo "export AXELAR_W=$wallet" >> $HOME/.bash_profile
echo "export AXELAR_CHAIN=$chain" >> $HOME/.bash_profile
echo "export AXELAR_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Init
```
sided init $SIDE_M --chain-id $SIDE_CHAIN --home $HOME/.side
```

### Download genesis
```
curl https://raw.githubusercontent.com/sideprotocol/testnet/main/side-testnet-3/genesis.json > $HOME/.side/config/genesis.json
```

### Address book
```

```

### Minimum gas prices 
```

```

### Pruning settings
```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "1000"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.side/config/app.toml
```

### Ports customizing
```
sed -i.bak \
-e "s%^proxy_app = \"tcp://127.0.0.1:[0-9]\{2\}658\"%proxy_app = \"tcp://127.0.0.1:${SIDE_PORT}658\"%;" \
-e "s%^laddr = \"tcp://127.0.0.1:[0-9]\{2\}657\"%laddr = \"tcp://127.0.0.1:${SIDE_PORT}657\"%;" \
-e "s%^pprof_laddr = \"localhost:[0-9]\{1\}060\"%pprof_laddr = \"localhost:${SIDE_PORT}060\"%;" \
-e "s%^laddr = \"tcp://0.0.0.0:[0-9]\{2\}656\"%laddr = \"tcp://0.0.0.0:${SIDE_PORT}656\"%;" \
-e "s%^prometheus_listen_addr = \":[0-9]\{2\}660\"%prometheus_listen_addr = \":${SIDE_PORT}660\"%" \
$HOME/.side/config/config.toml

sed -i.bak \
-e "s%^address = \"tcp://localhost:[0-9]\{1\}317\"%address = \"tcp://localhost:${SIDE_PORT}317\"%;" \
-e "s%^address = \":[0-9]\{1\}080\"%address = \":${SIDE_PORT}080\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}090\"%address = \"localhost:${SIDE_PORT}090\"%;" \
-e "s%^address = \"localhost:[0-9]\{1\}091\"%address = \"localhost:${SIDE_PORT}091\"%;" \
$HOME/.side/config/app.toml
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
