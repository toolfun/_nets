# Quasar

> - chain id: qsr-questnet-04    
> - binary version: v0.0.2-alpha-11



###
```

```


###
```

```


###
```
QUASAR_NODENAME=
QUASAR_WALLET=
QUASAR_PORT=
```


###
```
QUASAR_CHAIN_ID=qsr-questnet-04
```


###
```
echo "export QUASAR_NODENAME=$QUASAR_NODENAME" >> $HOME/.bash_profile
echo "export QUASAR_WALLET=$QUASAR_WALLET" >> $HOME/.bash_profile
echo "export QUASAR_CHAIN_ID=$QUASAR_CHAIN_ID" >> $HOME/.bash_profile
echo "export QUASAR_PORT=$QUASAR_PORT" >> $HOME/.bash_profile
source ~/.bash_profile
```


###
```
wget -O quasarnoded https://github.com/quasar-finance/binary-release/raw/main/v0.0.2-alpha-11/quasarnoded-linux-amd64
```


###
```
chmod +x quasarnoded
```


###
```
mv quasarnoded ~/go/bin/
```


###
```
quasarnoded init $QUASAR_NODENAME --chain-id $QUASAR_CHAIN_ID
```


###
```
curl https://raw.githubusercontent.com/quasar-finance/questnet/main/v04/definitive-genesis.json > ~/.quasarnode/config/genesis.json
```


###
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${QUASAR_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${QUASAR_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${QUASAR_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${QUASAR_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${QUASAR_PORT}660\"%" $HOME/.quasarnode/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${QUASAR_PORT}317\"%; s%^address = \":8080\"%address = \":${QUASAR_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${QUASAR_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${QUASAR_PORT}091\"%" $HOME/.quasarnode/config/app.toml
```


###
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.quasarnode/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.quasarnode/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.quasarnode/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.quasarnode/config/app.toml
```


###
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.quasarnode/config/config.toml
```


###
```
quasarnoded config keyring-backend test
quasarnoded config chain-id $QUASAR_CHAIN_ID
quasarnoded config node tcp://localhost:${QUASAR_PORT}657
```


###
```
sudo tee /etc/systemd/system/quasard.service > /dev/null << EOF
[Unit]
Description=Quasar Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which quasarnoded) start
Restart=on-failure
RestartSec=5
LimitNOFILE=65536
[Install]
WantedBy=multi-user.target
EOF
```


###
```
quasarnoded tendermint unsafe-reset-all --home $HOME/.quasarnode
```


### Snapshot
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
sudo systemctl daemon-reload
sudo systemctl enable quasard
sudo systemctl restart quasard
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
quasarnoded keys add $QUASAR_WALLET
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
