# Initia testnet ended

chain-id `initiation-1`    
binary `v0.2.23-stage-2`    

<!-- -------------------------------------------------
____

### Updating
```
sudo apt update && sudo apt upgrade -y
```

### Installing tools
```
sudo apt install curl jq git make libssl-dev build-essential bsdmainutils -y
```

### Go installing
```
ver="1.22.2" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

### Build binary
```
git clone https://github.com/initia-labs/initia
cd initia
git checkout v0.2.23-stage-2
make install
```

### Variables, set names
```
moniker=
wallet=
chain=initiation-1
```

### Variables, add to profile
```
echo "export INI_M=$moniker" >> $HOME/.bash_profile
echo "export INI_W=$wallet" >> $HOME/.bash_profile
echo "export INI_CHAIN=$chain" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Genesis
```
wget -O $HOME/.initia/config/genesis.json https://initia.s3.ap-southeast-1.amazonaws.com/initiation-1/genesis.json    
```

### Init
```
initiad init $INI_M --chain-id initiation-1     
```


pruning    
ports    
minimum-gas-prices



### Seeds
2eaa272622d1ba6796100ab39f58c75d458b9dbc@34.142.181.82:26656    
c28827cb96c14c905b127b92065a3fb4cd77d7f6@testnet-seeds.whispernode.com:25756

also check in Discord https://discord.com/invite/initia #validators

### Demon
```
sudo tee /etc/systemd/system/initiad.service > /dev/null <<EOF
[Unit]
Description=Initia_#T
After=network-online.target

[Service]
User=$USER
ExecStart=$(which initiad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Start
```
sudo systemctl daemon-reload && \
sudo systemctl enable initiad && \
sudo systemctl restart initiad && \
journalctl -fu initiad -o cat
```

### Add account
```
initiad keys add $INI_W
```

### Create validator (after syncing)
```
initiad tx mstaking create-validator \
    --amount=5000000uinit \   # It can be other LP tokens 
    --pubkey=$(initiad tendermint show-validator) \
    --moniker=$INI_M \
    --from=INI_W \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01"
    -y
```

# Oracle

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

-------------------------------------------------- -->
