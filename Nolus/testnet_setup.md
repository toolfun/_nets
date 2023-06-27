# 🚧 outdated

### Clone repo & install
```
git clone https://github.com/Nolus-Protocol/nolus-core
cd nolus-core
git checkout v0.1.39
make install
```

```
export NOLUS_NODENAME=""
export NOLUS_PORT=30
export NOLUS_WALLET=
export NOLUS_CHAIN_ID=nolus-rila
export NOLUS_WALLET_ADDRESS=
export NOLUS_VALOPER=
```

```
nolusd init $NOLUS_NODENAME --chain-id $NOLUS_CHAIN_ID
```

```
wget -O $HOME/.nolus/config/genesis.json "https://raw.githubusercontent.com/Nolus-Protocol/nolus-networks/main/testnet/nolus-rila/genesis.json"
```

```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${NOLUS_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${NOLUS_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${NOLUS_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${NOLUS_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${NOLUS_PORT}660\"%" $HOME/.nolus/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${NOLUS_PORT}317\"%; s%^address = \":8080\"%address = \":${NOLUS_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${NOLUS_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${NOLUS_PORT}091\"%" $HOME/.nolus/config/app.toml
```

```
tee /etc/systemd/system/nolusd.service > /dev/null <<EOF
[Unit]
Description=Nolus daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$(which nolusd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
