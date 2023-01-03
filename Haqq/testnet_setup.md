## Setup v1.3.0 

### Update system

```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev build-essential jq git make ncdu gcc chrony screen tmux htop -y
```

### Go

```
if ! [ -x "$(command -v go)" ]; then
  ver="1.19.4"
  cd $HOME
  wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
  sudo rm "go$ver.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
fi
```

### Variables
```
export HAQQ_NODENAME=
export HAQQ_WALLET=
export HAQQ_CHAIN_ID=haqq_54211-3
export HAQQ_PORT=33
export HAQQ_WALLET_ADDRESS=
export HAQQ_VALOPER=
```
```
. ~/.bash_profile
```

### Install
```
cd $HOME
git clone https://github.com/haqq-network/haqq
cd haqq
git checkout v1.3.0
make install 
```
> **Check version**    
```
haqqd version --long | head
```
> version: '"v1.3.0"'    
> commit: 39b4dcb706e72c187fc23ec6dddcfed20c79bc9d

### Configure
```
haqqd config chain-id $HAQQ_CHAIN_ID
haqqd config keyring-backend file
haqqd config node tcp://localhost:${HAQQ_PORT}657
```

### Initializing
```
haqqd init $HAQQ_NODENAME --chain-id $HAQQ_CHAIN_ID
```

### Genesis
```
wget -qO $HOME/.haqqd/config/genesis.json "https://raw.githubusercontent.com/haqq-network/validators-contest/master/genesis.json"
```

### Ports for haqqd
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${HAQQ_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${HAQQ_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${HAQQ_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${HAQQ_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${HAQQ_PORT}660\"%" $HOME/.haqqd/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${HAQQ_PORT}317\"%; s%^address = \":8080\"%address = \":${HAQQ_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${HAQQ_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${HAQQ_PORT}091\"%" $HOME/.haqqd/config/app.toml
```

### Prining
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="20"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.haqqd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.haqqd/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.haqqd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.haqqd/config/app.toml
```

### Indexer off
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.mande-chain/config/config.toml
```

### Minimum gas price
```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0aISLM\"/" $HOME/.haqqd/config/app.toml
```

### Reset chain
```
haqqd tendermint unsafe-reset-all --home $HOME/.haqqd
```

### Create service fo haqqd
```
sudo tee /etc/systemd/system/haqqd.service > /dev/null <<EOF
[Unit]
Description=Haqq_Node
After=network-online.target

[Service]
User=$USER
ExecStart=$(which haqqd) start --home $HOME/.haqqd
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### Address book
```

```

### Chose one option how to start. Start with 1. state sync ❗OR 2. snapshot
- 1. State sync    
```

```
```
sudo systemctl daemon-reload
sudo systemctl enable haqqd
sudo systemctl restart haqqd && sudo journalctl -u haqqd -f -o cat
```
- 2. Snapshot    
```

```
```
sudo systemctl daemon-reload
sudo systemctl enable haqqd
sudo systemctl restart haqqd && sudo journalctl -u haqqd -f -o cat
```


### Add OR recover wallet
- Add    
```
haqqd keys add $HAQQ_WALLET
```
- Recover    
```
haqqd keys add $HAQQ_WALLET --recover
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
