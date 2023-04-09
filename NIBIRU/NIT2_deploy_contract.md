## Nibiru Incentivized Testnet Phase2
#### Tutorial for the task: Deploy a smart contract on Nibiru
Based on the official manual https://nibiru.fi/blog/posts/010-itn-2-cosmwasm-governance.html

#

#### 📌 This manual is for the case if you have not installed nibiru node or want to do the task on another server
### Install the Nibiru binary
```
curl -s https://get.nibiru.fi/! | bash
```

### Config the app
```
nibid config node https://rpc.itn-1.nibiru.fi:443
nibid config chain-id nibiru-itn-1
nibid config broadcast-mode block
```

### Make a working  directory
```
mkdir ~/nibiru_contract
cd ~/nibiru_contract
```

### Download a smart contract's .wasm binary: cw1_whitelist.wasm
(Repository https://github.com/NibiruChain/cw-nibiru/tree/main/artifacts-cw-plus)
```
wget https://github.com/NibiruChain/cw-nibiru/raw/main/artifacts-cw-plus/cw1_whitelist.wasm
```

### Add your wallet, which you registered to participate in the testnet to make the task count
add prefered name
```
KEY_NAME=
```
recover with a seed phrase
```
nibid keys add $KEY_NAME --recover
```

### Make variable for the dowloaded contract file
```
CONTRACT_WASM=cw1_whitelist.wasm
```

### Deploy the contract
```
nibid tx wasm store $CONTRACT_WASM --from $KEY_NAME --gas=2000000 --fees=50000unibi -y > my_wasm_store.json
```

### Get the `txhash`
from the output, at the very bottom:    
![](https://github.com/toolfun/_pics/blob/main/nbtxhashexmp.jpg)    

OR get it from the file:
```
cat my_wasm_store.json | grep "txhash" | cut -d ':' -f2 | tr -d ' '
```

### Get the `code_id`
from the output, at the bottom:    
![](https://github.com/toolfun/_pics/blob/main/nbcodeexmp.jpg)

from the file
```
cat my_wasm_store.json | grep "code_id" | cut -d ':' -f17 | tr -d ' "|,}]' | sed 's/.$//' | sed '/^$/d'
```
OR from the chain:
```
hash=$(cat my_wasm_store.json | grep "txhash" | cut -d ':' -f2 | tr -d ' ')
nibid q tx --type=hash $hash -oj | jq -r '.logs[].events[] | select(.type == "store_code") | .attributes[] | select(.key == "code_id") | .value'
```

### Query
XXXX replace with the `code_id`
```
nibid query wasm code-info XXXX
```

____

## Instantiate a smart contract

### 
```

```







____

## Done
