# Empower Chain node setup
Chain: empowerchain-1    
GitHub reference: https://github.com/EmpowerPlastic/empowerchain/tree/main/mainnet/empowerchain-1
____

### General updating
```
sudo apt update && sudo apt upgrade -y
```

### Go version > 1.20
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

### Add variables to env
```bash
# any moniker
export EMP_M=

# any keyname
export EMP_K=

# chain-id is preset
export EMP_CHAIN=empowerchain-1

# customize ports, any 2 didgits
export EMP_PORT=
```
```
echo "export EMP_M=$EMP_M" >> $HOME/.bash_profile
echo "export EMP_K=$EMP_K" >> $HOME/.bash_profile
echo "export EMP_CHAIN=$EMP_CHAIN" >> $HOME/.bash_profile
echo "export EMP_PORT=$EMP_PORT" >> $HOME/.bash_profile
```
```
source $HOME/.bash_profile
```

### Install empowerd
```
git clone https://github.com/EmpowerPlastic/empowerchain && \
cd empowerchain && \
git checkout v1.0.0 && \
cd chain && \
make install
```

- Verify empowerd version
```bash
empowerd version --long | grep -e version -e commit
```
version: 1.0.0    
commit: 5d80d3c26256d9809cbd0b4dacfd0a8dbcaacc95

### Init dir
```
empowerd init "$EMP_M" --chain-id $EMP_CHAIN
```

### Config app
```
empowerd config keyring-backend file
empowerd config chain-id $EMP_CHAIN
empowerd config node tcp://localhost:${EMP_PORT}657
```

### Genesis
```
URL=https://github.com/EmpowerPlastic/empowerchain/raw/main/mainnet/empowerchain-1/genesis.tar.gz
curl -L $URL | tar -xz -C $HOME/.empowerchain/config/
```
- Verify genesis
```
sha256sum $HOME/.empowerchain/config/genesis.json
```
Result: 819d33d14c35bbfbc5997db9bf545eb7a5504b5870a307ce90c3813add4b316b

### Indexer (optional)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.empowerchain/config/config.toml
```

### Ports (optional)
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${EMP_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${EMP_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${EMP_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${EMP_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${EMP_PORT}660\"%" $HOME/.empowerchain/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${EMP_PORT}317\"%; s%^address = \":8080\"%address = \":${EMP_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${EMP_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${EMP_PORT}091\"%" $HOME/.empowerchain/config/app.toml
```

### Pruning (optional)
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.empowerchain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.empowerchain/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.empowerchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.empowerchain/config/app.toml
```

### Minimum gas price
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025umpwr\"/" $HOME/.empowerchain/config/app.toml
```

### Reset state
```
empowerd tendermint unsafe-reset-all --home $HOME/.empowerchain --keep-addr-book
```

### Create service
```
sudo tee /etc/systemd/system/empowerd.service > /dev/null << EOF
[Unit]
Description=Empower node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which empowerd) start
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Address book, seeds, peers
```
seeds="a1427b456513ab70967a2a5c618d347bc89e8848@seed.empowerchain.io:26656,6740fa259552a628266a85de8c2a3dee7702b8f9@empower-mainnet-seed.itrocket.net:14656,e16668ddd526f4e114ebb6c4714f0c18c0add8f8@empower-seed.zenscape.one:26656,f2ed98cf518b501b6d1c10c4a16d0dfbc4a9cc98@tenderseed.ccvalidators.com:27001"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|' $HOME/.empowerchain/config/config.toml
```

### (Snapshot) Start
```

```

### Add OR recover wallet
```

```

### Variables with your wallet and valoper addresses
```

```

### Tokens 
```

```

### Create validator
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
