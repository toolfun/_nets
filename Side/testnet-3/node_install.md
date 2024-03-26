`side-testnet-3` `v0.7.0`
# Side testnet 3

### System update
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### Go installation
```
ver="1.21.5"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
sudo rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
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

### Service file
```
sudo tee /etc/systemd/system/sided.service > /dev/null <<EOF
[Unit]
Description=Sided_#T_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which sided) start --home $HOME/.side
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Add seed node
```
sed -i 's/seeds = ""/seeds = "00170c0c23c3e97c740680a7f881511faf68289a@202.182.119.24:26656"/' ~/.side/config/config.toml
```

###
```
sided tendermint unsafe-reset-all --home $HOME/.side --keep-addr-book
```

### Start the node
```
sudo systemctl daemon-reload && \
sudo systemctl enable --now sided && sudo journalctl -u sided -f --no-hostname -o cat
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
