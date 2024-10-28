### `chiado_10010-1`

#### `node setup` `validator setup` `oracle setup`

#

### Installing tools
```bash
sudo apt update
sudo apt install make gcc curl wget git jq build-essential lz4 unzip -qy
sudo apt upgrade
```

### Install Go 1.22.3 or later
```bash
sudo rm -rf /usr/local/go
v="1.22.3"
wget "https://golang.org/dl/go$v.linux-amd64.tar.gz"
sudo tar -C /usr/local -xzf "go$v.linux-amd64.tar.gz"
rm "go$v.linux-amd64.tar.gz"
```

### Set variables: moniker for your validator node, wallet for wallet name. The chain leave as is
```bash
moniker=
wallet=
chain="chiado_10010-1"
```

### You can customize which ports will be used, or do nothing and leave as default.
For example set
```bash
port=15
```

### Load variables
```bash
echo "export WARDEN_MONIKER=$moniker" >> $HOME/.bash_profile
echo "export WARDEN_WALLET=$wallet" >> $HOME/.bash_profile
echo "export WARDEN_CHAIN=$chain" >> $HOME/.bash_profile
echo "export WARDEN_PORT=$port" >> $HOME/.bash_profile
source ~/.bash_profile
```

### Getting Warden binary
```bash
mkdir -p ~/wardenprotocol && cd ~/wardenprotocol
wget https://github.com/warden-protocol/wardenprotocol/releases/download/v0.5.2/wardend_Linux_x86_64.zip
unzip wardend_Linux_x86_64.zip
chmod +x wardend
mv $HOME/wardenprotocol/wardend $HOME/go/bin
```

### Check version
```bash
wardend version --long | grep -e version -e commit
```

### Init
```bash
wardend init $WARDEN_MONIKER --chain-id $WARDEN_CHAIN
```

### Download Genesis file
```bash
wget -O $HOME/.warden/config/genesis.json "https://raw.githubusercontent.com/warden-protocol/networks/main/testnets/chiado/genesis.json"
```

### Address books (optional)
You can find addrbooks or ask for it, if you have issues with syncing, in Warden Protocol Discord https://discord.gg/wardenprotocol    
- itrocket.net
```bash
wget -O $HOME/.warden/config/addrbook.json https://server-4.itrocket.net/testnet/warden/addrbook.json
```
- UTSA
```bash
wget -O $HOME/.warden/config/addrbook.json "https://share.utsa.tech/warden/addrbook.json"
```

###
```bash
```

###
```bash
```

###
```bash
```
