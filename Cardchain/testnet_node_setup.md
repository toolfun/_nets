### DecentralCardGame - Crowd Control
# 🚧🚧🚧 under construction
<!-- # ############################################## Cardchain testnet node installation -->
- Docs: https://github.com/DecentralCardGame/Testnet
- Explorer: https://explorer.stavr.tech/cardchain
- Explorer: https://explorer.kjnodes.com/cardchain-testnet
- Discord: https://discord.gg/xvuEfcaUd6

____


### https://go.dev/doc/install
You can change version under the v variable
```
sudo rm -rf /usr/local/go
v="1.21.0"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```
Create .bash_profile file and update $PATH
```
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
```

### Set variables
Think up and write a `moniker` (nodename), name of the `wallet` and change `port` (you can put your own number). Keep the `chain` intact 
```
moniker=
wallet=
port=11
chain=Testnet3
```

### Write and load variables
```
echo "export CARDCHAIN_M=$moniker" >> $HOME/.bash_profile
echo "export CARDCHAIN_W=$wallet" >> $HOME/.bash_profile
echo "export CARDCHAIN_CHAIN=$chain" >> $HOME/.bash_profile
echo "export CARDCHAIN_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Download CC binary
```
wget https://github.com/DecentralCardGame/Cardchain/releases/download/v0.81/Cardchain_latest_linux_amd64.tar.gz
tar xzf Cardchain_latest_linux_amd64.tar.gz
chmod 775 Cardchaind
sudo mv Cardchaind /usr/local/bin/
sudo rm Cardchain_latest_linux_amd64.tar.gz
```

### Config client
```
Cardchaind config chain-id $CARDCHAIN_CHAIN
Cardchaind config keyring-backend test
Cardchaind config node tcp://localhost:${CARDCHAIN_PORT}657
Cardchaind init $CARDCHAIN_M --chain-id $CARDCHAIN_CHAIN
```

### Download genesis
```
wget -O $HOME/.Cardchain/config/genesis.json https://github.com/DecentralCardGame/Testnet/raw/main/genesis.json
```

### Config indexer (off) *optional*
```
indexer="null" sed -i -e "s/^indexer =./indexer = "$indexer"/" $HOME/.Cardchain/config/config.toml
```

### Changing custom ports *optional*
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CARDCHAIN_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CARDCHAIN_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CARDCHAIN_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CARDCHAIN_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CARDCHAIN_PORT}660\"%" $HOME/.Cardchain/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CARDCHAIN_PORT}317\"%; s%^address = \":8080\"%address = \":${CARDCHAIN_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CARDCHAIN_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CARDCHAIN_PORT}091\"%" $HOME/.Cardchain/config/app.toml
```

### Config prunings *optional*
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.Cardchain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.Cardchain/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.Cardchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.Cardchain/config/app.toml
```

### Snapshot or state sync
Search in the Crowd Control Discord https://discord.gg/xvuEfcaUd6 or ask the community


### Service file for Cardchaind
```
sudo tee /etc/systemd/system/Cardchaind.service > /dev/null <<EOF
[Unit]
Description=Cardchain_node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which Cardchaind) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

```

### Start Cardchain
```
sudo systemctl daemon-reload
sudo systemctl enable --now Cardchaind && sudo journalctl -u Cardchaind -f -o cat
```

### Add new or existing wallet
New
```
Cardchaind keys add $CARDCHAIN_W
```
Existing
```
Cardchaind keys add $CARDCHAIN_W --recover
```

### Create validator
```
Cardchain tx staking create-validator \
  --from=$CARDCHAIN_W \
  --amount=1000000ubpf \
  --moniker=$CARDCHAIN_M \
  --chain-id=$CARDCHAIN_CHAIN \
  --commission-rate=0.1 \
  --commission-max-rate=0.5 \
  --commission-max-change-rate=0.1 \
  --min-self-delegation=1 \
  --pubkey=$(Cardchaind tendermint show-validator) \
  --yes
```

____


### Delete Cardchain node
```bash
# Deleting service
sudo systemctl stop Cardchaind
sudo rm /etc/systemd/system/Cardchaind.service

# Deleting work folder folder
sudo rm -r $HOME/.Cardchain/

# Deleting binary
sudo rm /usr/local/bin/Cardchaind
```

