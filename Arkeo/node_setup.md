# Arkeo

### updating
```
sudo apt update
sudo apt install make gcc curl wget git jq lz4 build-essential screen nano ncdu -qy
sudo apt upgrade
```

### create user file
```
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
```

### go
```
sudo rm -rf /usr/local/go
v="1.21.0"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```

### customizing
Set variables `moniker` for your validator node, `wallet` name. The `port` can be changed, `chain` is not
```
moniker=
wallet=
port=18
chain=
```
Load variables
```
echo "export ARKEO_M=$moniker" >> $HOME/.bash_profile
echo "export ARKEO_W=$wallet" >> $HOME/.bash_profile
echo "export ARKEO_CHAIN=$chain" >> $HOME/.bash_profile
echo "export ARKEO_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### insatlling Arkeo
```
cd
git clone https://github.com/arkeonetwork/arkeo
cd arkeo
git checkout ab05b124336ace257baa2cac07f7d1bfeed9ac02
make proto-gen install
arkeod version
```

### config
```
arkeod keys add <key-name>
arkeod config chain-id arkeo
```

### init
```
arkeod init <your_custom_moniker> --chain-id arkeo
```

### genesis
```
curl -s http://seed.arkeo.network:26657/genesis | jq '.result.genesis' > ~/.arkeo/config/genesis.json
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

###
```

```
