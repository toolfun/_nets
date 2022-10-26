# EmpowerChain testnet. Install guide

## Links

> <a href=https://www.empowerchain.io/>site</a>
> 
> <a href=https://medium.com/empowerplastic/>Medium</a>
> 
> <a href=https://github.com/empowerchain/>GitHub</a>
> 
> <a href=https://discord.gg/STr4RjmZ>Discord</a>
> 
> <a href=https://explorer.nodestake.top/empower-testnet>NodeStake explorer</a>
> 
> <a href=https://testnet-empower.zenscan.io/index.php>Zenscan explorer</a>
> 
> <a href=https://empower.explorers.guru/>NG explorer</a>

#

### Set up `variables`

Put your moniker (nodename) - same is your validator name. And name your wallet (account)
```
EMP_NODENAME=<YOUR_NODENAME>
```
```
EMP_WALLET=<YOUR_WALLET_NAME>
```
```
EMP_PORT=38
```
You can choose any other (available) port number. The application ports will be configured down below as follows: `26`657 -> `38`657 ...etc

### Save your `port`, your `wallet name` and preset `chain id` as variables
```
echo "export EMP_NODENAME=$EMP_NODENAME" >> $HOME/.bash_profile
echo "export EMP_WALLET=$EMP_WALLET" >> $HOME/.bash_profile
echo "export EMP_CHAIN_ID=altruistic-1" >> $HOME/.bash_profile
echo "export EMP_PORT=${EMP_PORT}" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### General `update`
```
sudo apt update && sudo apt upgrade -y
```

### Install `tools`
```
sudo apt install curl build-essential git wget jq make gcc tmux chrony -y
```

### Install `Go`
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

### Download and `install` EmpowerChain
```
cd $HOME && git clone https://github.com/empowerchain/empowerchain && \
cd empowerchain/chain && \
make install && \
```

### Check `version`
```
empowerd version --long | head
```

### `Configure` application
```
empowerd config chain-id $EMP_CHAIN_ID
empowerd config keyring-backend test
empowerd config node tcp://localhost:${EMP_PORT}657
```

### Make `init`
```
empowerd init $EMP_NODENAME --chain-id $EMP_CHAIN_ID
```

### Download `genesis`
```
wget -O $HOME/.empowerchain/config/genesis.json https://raw.githubusercontent.com/empowerchain/empowerchain/main/testnets/altruistic-1/genesis.json
```

### Set `seeds and peers`. You can ask for actual peers and addressbook in Discord
```
seeds=""
peers="9de92b545638f6baaa7d6d5109a1f7148f093db3@65.108.77.106:26656,4fd5e497563b2e09cfe6f857fb35bdae76c12582@65.108.206.56:26656,fe32c17373fbaa36d9fd86bc1146bfa125bb4f58@5.9.147.185:26656,220fb60b083bc4d443ce2a7a5363f4813dd4aef4@116.202.236.115:26656,225ad85c594d03942a026b90f4dab43f90230ea0@88.99.3.158:26656,2a2932e780a681ddf980594f7eacf5a33081edaf@192.168.147.43:26656,333de3fc2eba7eead24e0c5f53d665662b2ba001@10.132.0.11:26656,4a38efbae54fd1357329bd583186a68ccd6d85f9@94.130.212.252:26656,52450b21f346a4cf76334374c9d8012b2867b842@167.172.246.201:26656,56d05d4ae0e1440ad7c68e52cc841c424d59badd@192.168.1.46:26656,6a675d4f66bfe049321c3861bcfd19bd09fefbde@195.3.223.204:26656,1069820cdd9f5332503166b60dc686703b2dccc5@138.201.141.76:26656,277ff448eec6ec7fa665f68bdb1c9cb1a52ff597@159.69.110.238:26656,3335c9458105cf65546db0fb51b66f751eeb4906@5.189.129.30:26656,bfb56f4cb8361c49a2ac107251f92c0ea5a1c251@192.168.1.177:26656,edc9aa0bbf1fcd7433fcc3650e3f50ab0becc0b5@65.21.170.3:26656,d582bcd8a8f0a20c551098571727726bc75bae74@213.239.217.52:26656,eb182533a12d75fbae1ec32ef1f8fc6b6dd06601@65.109.28.219:26656,b22f0708c6f393bf79acc0a6ca23643fe7d58391@65.21.91.50:26656,e8f6d75ab37bf4f08c018f306416df1e138fd21c@95.217.135.41:26656,ed83872f2781b2bdb282fc2fd790527bcb6ffe9f@192.168.3.17:26656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.empowerchain/config/config.toml
```

### Set custom `ports`
> $EMP_PORT setted for 38
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${EMP_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${EMP_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${EMP_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${EMP_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${EMP_PORT}660\"%" $HOME/.empowerchain/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${EMP_PORT}317\"%; s%^address = \":8080\"%address = \":${EMP_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${EMP_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${EMP_PORT}091\"%" $HOME/.empowerchain/config/app.toml
```

### Configure custom `pruning`
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.empowerchain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.empowerchain/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.empowerchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.empowerchain/config/app.toml
```

### Set `indexer` in the null state
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.empowerchain/config/config.toml
```

### Set `minimum gas price`
```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0umpwr\"/" $HOME/.empowerchain/config/app.toml
```

### Enable `prometheus`
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.empowerchain/config/config.toml
```

### `Reset` chain data
empowerd tendermint unsafe-reset-all --home $HOME/.empowerchain
```

### Create `service`
```
sudo tee /etc/systemd/system/empowerd.service > /dev/null <<EOF
[Unit]
Description=EmpowerChain Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which empowerd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Register and `start` service
```
sudo systemctl daemon-reload
sudo systemctl enable empowerd
sudo systemctl restart empowerd && sudo journalctl -u empowerd -f -o cat
```
### Check `synchronization` status
```
empowerd status 2>&1 | jq .SyncInfo
```
> `cathing_up: true` means still syncing    
> `false` means synced, you can create validator    

> this timestamp, as shown, means that your node is unable to start syncing. Add actual peers to config.toml or repalce addrbook    

![](https://github.com/toolfun/pics/blob/main/1970sync.jpg)

### Add `wallet`
```
empowerd keys add $EMP_WALLET
```
Or recover it
```
empowerd keys add $EMP_WALLET --recover
```
To fund wallet go to the <a href="https://discord.com/channels/948213834164883488/1026598604523180043">faucet</a> channel in Discord


### Add wallet and valoper addresses as `variables`
```
EMP_WALLET_ADDRESS=$(empowerd keys show $EMP_WALLET -a)
EMP_VALOPER_ADDRESS=$(empowerd keys show $EMP_WALLET --bech val -a)
echo 'export EMP_WALLET_ADDRESS='${EMP_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export EMP_VALOPER_ADDRESS='${EMP_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Create `validator`
> `commission-max-change-rate` and `commission-max-rate` once set it can't be modidfied
```
empowerd tx staking create-validator \
  --amount 1000000umpwr \
  --from $EMP_WALLET \
  --commission-max-change-rate "0.1" \
  --commission-max-rate "0.2" \
  --commission-rate "0.09" \
  --min-self-delegation "1" \
  --pubkey $(empowerd tendermint show-validator) \
  --moniker $EMP_NODENAME \
  --chain-id $EMP_CHAIN_ID
  ```

# Usefull stuff
### Disable and cleanup `indexing`
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.empowerchain/config/config.toml
sudo systemctl restart empowerd
sleep 3
sudo rm -rf $HOME/.empowerchain/data/tx_index.db
```

### Edit or add `validator` info
```
haqqd tx staking edit-validator \
  --moniker=$EMP_NODENAME \
  --identity=<your keybase id to set avatar> \
  --website="<your website>" \
  --details="<some descriptions>" \
  --chain-id=$EMP_CHAIN_ID \
  --from=$EMP_WALLET
```

### Last `synced` block
```
empowerd status 2>&1 | jq .SyncInfo.latest_block_height
```

### `Synced` or not
```
empowerd status 2>&1 | jq .SyncInfo.catching_up
```
> `false` = synced, `true` = still syncing
