### MANDE Chain setup. Testnet-1
____

# ⚠️ I mark it as a scam. Proof link: [Mande team announcement](https://discord.com/channels/953348696098103366/1031290123033251930/1037014456191963156)

## links
- site https://www.mande.network/
- github https://github.com/mande-labs/testnet-1
- discord https://discord.gg/zbDMQEcp
- explorer https://explorer.stavr.tech/mande-chain

### general
```
sudo apt update && sudo apt install curl build-essential git wget jq make gcc tmux chrony screen ncdu htop clang libssl-dev -y
```

### install go
```
if ! [ -x "$(command -v go)" ]; then
  ver="1.19.2"
  cd $HOME
  wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
  rm "go$ver.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
fi
```

### variables
```
MANDE_NODENAME=<your>
MANDE_WALLET=<your>
MANDE_PORT=34
MANDE_CHAIN_ID=mande-testnet-1
```

```
echo "export MANDE_NODENAME=$MANDE_NODENAME" >> $HOME/.bash_profile
echo "export MANDE_WALLET=$MANDE_WALLET" >> $HOME/.bash_profile
echo "export MANDE_CHAIN_ID=$MANDE_CHAIN_ID" >> $HOME/.bash_profile
echo "export MANDE_PORT=${MANDE_PORT}" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### download binary
```
curl -OL https://github.com/mande-labs/testnet-1/raw/main/mande-chaind
mv mande-chaind $HOME/go/bin/
chmod 744 $HOME/go/bin/mande-chaind
mande-chaind version --long | head
```
> Check it
```
mande-chaind version --long | head
```

> Must be    
`version: 1.3-00086e89`    
`commit: 00086e892d5747326b3c13808604422ad4428ffa`

### init
```
mande-chaind init $MANDE_NODENAME --chain-id $MANDE_CHAIN_ID
```

### genesis
```
wget -O $HOME/.mande-chain/config/genesis.json "https://raw.githubusercontent.com/mande-labs/testnet-1/main/genesis.json"
```

### client config
```
mande-chaind config chain-id $MANDE_CHAIN_ID
mande-chaind config keyring-backend test
mande-chaind config node tcp://localhost:${MANDE_PORT}657
```

### ports
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${MANDE_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${MANDE_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${MANDE_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${MANDE_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${MANDE_PORT}660\"%" $HOME/.mande-chain/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${MANDE_PORT}317\"%; s%^address = \":8080\"%address = \":${MANDE_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${MANDE_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${MANDE_PORT}091\"%" $HOME/.mande-chain/config/app.toml
```

### pruning
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.mande-chain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.mande-chain/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.mande-chain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.mande-chain/config/app.toml
```

### indexer
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.mande-chain/config/config.toml
```

### min gas price
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.005mand\"/;" ~/.mande-chain/config/app.toml
```

### peers (upd. 17-10-2022)
```
peers="bd9929b9a2e8b5ad1581e4b01f85457e0d01cba3@209.182.239.169:28656,f9dec9a209fdcb22339d87eaadffde97d3ecf648@45.67.216.40:26656,c2ec4f71950d1d4e6233ed450b09f08d15ffbe98@195.201.165.123:10086,4fbbf09c5561c4a9692e368a672b99180b3f70ee@185.182.184.200:46656,156eb9c408b5274c14e7139fa14b3210de359848@5.161.113.160:26656,074a8eaf817da9df97c5becf367baaf2f3e1917f@135.125.163.63:26666,19a7467dc9aa99b3cdc8ee82492a57c4ffa46fc3@5.161.98.239:26656,2365cf1278df6bdf26b314d4f9c4e4108734b51d@144.126.156.253:26656,a3e3e20528604b26b792055be84e3fd4de70533b@38.242.199.93:24656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.mande-chain/config/config.toml
```

### timeout_commit
```
sed -i -e "s/^timeout_commit *=.*/timeout_commit = \"2s\"/" $HOME/.mande-chain/config/config.toml
```
### addrbook (upd. 19.10.2022)
```
wget -O $HOME/.mande-chain/config/addrbook.json "https://raw.githubusercontent.com/toolfun/MANDE-setup/main/addrbook.json"
```
### unsafe reset data
```
mande-chaind tendermint unsafe-reset-all --home /root/.mande-chain
```

### service
```
sudo tee /etc/systemd/system/mande-chaind.service > /dev/null <<EOF
[Unit]
Description=Mande Daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$(which mande-chaind) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

```
systemctl daemon-reload && \
systemctl enable mande-chaind && \
systemctl restart mande-chaind && journalctl -u mande-chaind -f -o cat
```

### wallet add
```
mande-chaind keys add $MANDE_WALLET
```
> Or recover existing wallet
```
mande-chaind keys add $MANDE_WALLET --recover
```

### add adresses to the system
```
MANDE_WALLET_ADDRESS=$(mande-chaind keys show $MANDE_WALLET -a)
MANDE_VALOPER_ADDRESS=$(mande-chaind keys show $MANDE_WALLET --bech val -a)
```
```
echo 'export MANDE_WALLET_ADDRESS='${MANDE_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export MANDE_VALOPER_ADDRESS='${MANDE_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### faucet
```
curl -d '{"address":"$MANDE_WALLET_ADDRESS"}' -H 'Content-Type: application/json' http://35.224.207.121:8080/request
```
   [discord faucet](https://discord.com/channels/953348696098103366/1033430536129101904/)    

### sync status
```
mande-chaind status | jq .SyncInfo
```
> `cathing_up: true` means still syncing    
> `false` means synced, you can create validator    

> this timestamp, as shown, means that your node is unable to start syncing. Add actual peers to config.toml or repalce addrbook.json file with actual peers    

![](https://github.com/toolfun/pics/blob/main/1970sync.jpg)

### create validator (after syncing)
```
mande-chaind tx staking create-validator \
--amount 0cred \
--pubkey "$(mande-chaind tendermint show-validator)" \
--from $MANDE_WALLET \
--moniker $MANDE_NODENAME \
--fees 1000mand \
--yes
```

## DELEGATING
> ### Using web interface
### cast via web UI
This is not the usual delegation process called **Voting**. Thus, you delegate your tokens    
- Go to http://35.224.207.121/    
- Connect Keplr    
- and push `Claim MANDE`    
> To delegate to your or to any other validator (increase vote power) you need to know his wallet address (validator account address, not valoper)   

![vote](https://github.com/toolfun/pics/blob/main/mande-vote-2.jpg)

- Push `Submit`, done    
> You can find more explanation in that Discord post
https://discord.com/channels/953348696098103366/1030760553683099648/1030794179997077565

### uncast via web UI
You can redeem your votes from the selected validator by switching to `Uncast`    
![vote](https://github.com/toolfun/pics/blob/main/mande-vote-3.jpg)

> ### Using command line interface
### cast uncast vote via commant line:
```
mande-chaind --from {{KEY_NAME}} --chain-id mande-testnet-1 tx voting create-vote [validator_address_to_vote] [amount] [mode]
```
> `amount` can be positive or negative (make sure the amount is between -1000000000 and 1000000000)    
> `mode` - 1 for cast, 0 for uncast

- Cast example: 
```
mande-chaind --from {{KEY_NAME}} --chain-id mande-testnet-1 tx voting create-vote mande... 10000000 1
```
- Uncast example: 
```
- mande-chaind --from {{KEY_NAME}} --chain-id mande-testnet-1 tx voting create-vote mande... 10000000 0
```
____
    
## ADDITIONAL
### about jail
❌ Un-jailing is not supported in the current TestNet    
✔️ If you are jalied. You can create a new one and then DM admin `Pratik | Mande Labs#6328`, he will asign your old cred back to you
### knowing your peer ID :globe_with_meridians:    
```
echo "$(mande-chaind tendermint show-node-id)@$(curl ifconfig.me):${MANDE_PORT}656"
```

### state sync
#####    
#### 2 options. If one does not work try another one    

**1.** :point_right:  **state sync opt1** thanks to [STAVR](https://github.com/obajay/)
```
SNAP_RPC=https://mande-testnet-rpc.jambulmerah.dev:443
```
```
peers="a3e3e20528604b26b792055be84e3fd4de70533b@38.242.199.93:24656" 
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.mande-chain/config/config.toml
```
```
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 500)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```
```
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.mande-chain/config/config.toml
```
```
mande-chaind tendermint unsafe-reset-all --home /root/.mande-chain --keep-addr-book
sudo systemctl restart mande-chaind && journalctl -u mande-chaind -f -o cat
```
#####    
**2.** :point_down: **state sync opt2** down below. Thanks to [SECARD](http://secard.info/)
```
SNAP_RPC=http://209.182.239.169:28657
```
```
peers="bd9929b9a2e8b5ad1581e4b01f85457e0d01cba3@209.182.239.169:28656"
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" ~/.mande-chain/config/config.toml
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
```
> Check. There must be 3 different values
```
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```
> Set it
```
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.mande-chain/config/config.toml
```
> Reset and restart
```
mande-chaind tendermint unsafe-reset-all --home /root/.mande-chain
systemctl restart mande-chaind && journalctl -u mande-chaind -f -o cat
```




