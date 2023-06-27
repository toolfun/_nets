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
```
export EMP_M=
export EMP_K=
export EMP_CHAIN=empowerchain-1
export EMP_PORT=
```
source ~/.bash_profile 

### Install empowerd
```
git clone https://github.com/EmpowerPlastic/empowerchain && \
cd empowerchain && \
git checkout v1.0.0 && \
cd chain && \
make install
```

### Verify empowerd version
```bash
empowerd version --long | grep -e version -e commit
```
Result:    
version: 1.0.0    
commit: 5d80d3c26256d9809cbd0b4dacfd0a8dbcaacc95

### Inint dir
```
empowerd init "$EMP_M" --chain-id $EMP_CHAIN
```

### Genesis
```
URL=https://github.com/EmpowerPlastic/empowerchain/raw/main/mainnet/empowerchain-1/genesis.tar.gz
curl -L $URL | tar -xz -C $HOME/.empowerchain/config/
```

### Indexer (optional)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.empowerchain/config/config.toml
```

### Ports (optional)
```

```

### Pruning (optional)
```

```

### Minimum gas price
```

```

### Reset chain
```

```

### Create service
```

```

### Address book, seeds, peers
```

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
