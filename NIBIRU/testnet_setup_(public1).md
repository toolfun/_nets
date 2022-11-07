## 🚧🚧🚧under construction
____

> ### 🧰 aid tools    
>- 📖 [addrbook.json](https://github.com/toolfun/nets/blob/main/NIBIRU/addrbook.json)    
>- 🚅 team peer `fbae8695e3fe191063b0b11e1a50f569e645acb2@34.139.206.22:26656`
____

### Links
- [Website](https://nibiru.fi/)
- [Twitter](https://twitter.com/NibiruChain)
- [Discord](https://discord.gg/nibiru)
- [Blog](https://blog.nibiru.fi/)
- [Docs](https://docs.nibiru.fi/)
- [Explorer1](https://testnet-1.nibiru.fi/)
- [Explorer2](https://nibiru.explorers.guru/)

### Minimum hardware requirements
- 2CPU
- 4GB RAM
- 100GB of disk space (SSD)
____

### Set variables

```
NIBIRU_NODENAME=<your_nodename>
```
```
NIBIRU_NODENAME=""
NIBIRU_PORT=32
NIBIRU_WALLET=
NIBIRU_CHAIN_ID=nibiru-testnet-1
NIBIRU_WALLET_ADDRESS=
NIBIRU_VALOPER=
```
#### In case if you'll set password protect with keyring-backend and some autotools
```
NIBIRU_PASSWORD=
```
```
echo 'export NIBIRU_NODENAME='\"${NIBIRU_NODENAME}\" >> $HOME/.bash_profile
echo 'export NIBIRU_PASSWORD='\"${NIBIRU_PASSWORD}\" >> $HOME/.bash_profile
echo 'source $HOME/.bashrc' >> $HOME/.bash_profile
```

`nano $HOME/.bash_profile`   
`source $HOME/.bash_profile`     

# Update and upgrade
```
sudo apt update
sudo apt install make clang pkg-config libssl-dev build-essential git jq ncdu bsdmainutils htop -y < "/dev/null"
```

### Install Go
```
cd $HOME
wget -O go1.19.2.linux-amd64.tar.gz https://golang.org/dl/go1.19.2.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.19.2.linux-amd64.tar.gz && rm go1.19.2.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
```
#### check go
```
go version
```

### Download and build
```
cd $HOME
git clone https://github.com/NibiruChain/nibiru
cd nibiru
git checkout v0.15.0
make build
sudo mv ./build/nibid /usr/local/bin/
```

### Init
```
nibid init "$NIBIRU_NODENAME" --chain-id=nibiru-testnet-1
```

### Config client
```
nibid config chain-id $NIBIRU_CHAIN_ID
nibid config keyring-backend file
nibid config node tcp://localhost:${NIBIRU_PORT}657
```

### Ports
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${NIBIRU_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${NIBIRU_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${NIBIRU_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${NIBIRU_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${NIBIRU_PORT}660\"%" $HOME/.nibid/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${NIBIRU_PORT}317\"%; s%^address = \":8080\"%address = \":${NIBIRU_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${NIBIRU_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${NIBIRU_PORT}091\"%" $HOME/.nibid/config/app.toml
```
### Optimization and settings
```
sed -i.default 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025unibi"/g' $HOME/.nibid/config/app.toml
```
```
CONFIG_TOML="$HOME/.nibid/config/config.toml"
sed -i 's/timeout_propose =.*/timeout_propose = "100ms"/g' $CONFIG_TOML
sed -i 's/timeout_propose_delta =.*/timeout_propose_delta = "500ms"/g' $CONFIG_TOML
sed -i 's/timeout_prevote =.*/timeout_prevote = "100ms"/g' $CONFIG_TOML
sed -i 's/timeout_prevote_delta =.*/timeout_prevote_delta = "500ms"/g' $CONFIG_TOML
sed -i 's/timeout_precommit =.*/timeout_precommit = "100ms"/g' $CONFIG_TOML
sed -i 's/timeout_precommit_delta =.*/timeout_precommit_delta = "500ms"/g' $CONFIG_TOML
sed -i 's/timeout_commit =.*/timeout_commit = "1s"/g' $CONFIG_TOML
sed -i 's/skip_timeout_commit =.*/skip_timeout_commit = false/g' $CONFIG_TOML
sed -i.default "s/pruning *=.*/pruning = \"custom\"/g" $HOME/.nibid/config/app.toml
sed -i "s/pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/g" $HOME/.nibid/config/app.toml
sed -i "s/pruning-interval *=.*/pruning-interval = \"10\"/g" $HOME/.nibid/config/app.toml
```
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.nibid/config/config.toml
rm -rf $HOME/.nibid/data/tx_index.db
```

### Service file
```
echo "[Unit]
Description=Nibiru Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/usr/local/bin/nibid start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/nibid.service
sudo mv $HOME/nibid.service /etc/systemd/system
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```

### State sync
#### stop node
```
sudo systemctl stop nibid
nibid tendermint unsafe-reset-all --home $HOME/.nibid
```
```
SEEDS=""
PEERS="095cc77588be94bc2988b4dba86bfb001ec925ff@135.181.111.204:26656"
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.nibid/config/config.toml

SNAP_RPC="http://135.181.111.204:26657"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.nibid/config/config.toml
```
  
#### start node
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable nibid
sudo systemctl restart nibid
```
#### check logs
```
journalctl -u nibid -f -o cat
```
  
### disable State Sync after node synchronization
```
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1false|" $HOME/.nibid/config/config.toml
```

### Create validator
```
nibid tx staking create-validator \
--amount=1000000unibi \
--pubkey=$(nibid tendermint show-validator) \
--moniker="$NIBIRU_NODENAME" \
--chain-id=$NIBIRU_CHAIN_ID \
--commission-rate="0.06" \
--commission-max-rate="0.22" \
--commission-max-change-rate="0.05" \
--min-self-delegation="1000000" \
--from=$NIBIRU_WALLET \
--fees=5000unibi \
-y
```
