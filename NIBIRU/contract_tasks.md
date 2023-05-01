## Nibiru Incentivized Testnet 

#### Phase2. Manual for the tasks 1 - 3

1. Deploy smart contract on Nibiru    
2. Instantiate a smart contract    
3. Successfully broadcast an ExecuteContract transaction    

#### 📌 This manual is for the case if you have not installed nibiru node or want to do the task on a separate server
---

## 1. Deploy smart contract on Nibiru

### Install the Nibiru binary
```
curl -s https://get.nibiru.fi/! | bash
```

### Config the app
```
nibid config node https://rpc.itn-1.nibiru.fi:443 # the rpc node "https://rpc.itn-1.nibiru.fi:443" might not work, find another RPC
nibid config chain-id nibiru-itn-1
nibid config broadcast-mode block
```

### Make a working  directory
```
mkdir ~/nibiru_contract
cd ~/nibiru_contract
```

### Download a smart contract
(Repository https://github.com/NibiruChain/cw-nibiru/tree/main/artifacts-cw-plus)
```
wget https://github.com/NibiruChain/cw-nibiru/raw/main/artifacts-cw-plus/cw20_base.wasm
```

### Add your wallet, which you registered to participate in the testnet to make the task count
add prefered name, e.g. KEY_NAME=mywallet
```
KEY_NAME=
```
recover your wallet with a seed phrase
```
nibid keys add $KEY_NAME --recover
```

### Make variable for the dowloaded contract file
```
CONTRACT_WASM=cw20_base.wasm
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

### Add code_id variable
My code_id is 1968 so I input
```
code=1968
```

### Query
```
nibid query wasm code-info $code
```

____

## 2. Instantiate a smart contract

### Adding wallet address to variable
```
KEY_ADDRESS="$(nibid keys show $KEY_NAME -a)"
```

### Make your own 
`name` value    
`symbol` value. Must be from 3 to 12 characters

```
init='{"name":"xAlex","symbol":"xAlex","decimals":6,"initial_balances":[{"address":"$KEY_ADDRESS","amount":"2000000"}],"mint":{"minter":"$KEY_ADDRESS"},"marketing":{}}'
```

### And make your own
`--label` value 
```
nibid tx wasm instantiate $code $init --from $KEY_NAME --label "xAlex cwbase" --gas-adjustment 1.2 --gas auto  --fees 10000unibi --no-admin
```

#### output:
![](https://github.com/toolfun/_pics/blob/main/nbcntrexmp.jpg)


### Add contract address in a variable
```
CONTRACT=$(nibid query wasm list-contract-by-code $code --output json | jq -r '.contracts[-1]')
```

____

## 3. Successfully broadcast an ExecuteContract transaction

### Enter any address to which you want to broadcast tokens
I enter `recipient` nibi12su8wat7ks8yvfgqls6n9eyygjlx3r6d4ex9lq    
The `amount` can be different than 10
```
TRANSFER='{"transfer":{"recipient":"nibi12su8wat7ks8yvfgqls6n9eyygjlx3r6d4ex9lq","amount":"10"}}'
```

### 
```
nibid tx wasm execute $CONTRACT $TRANSFER --from $KEY_NAME --gas 200000 --gas-adjustment 1.2 --fees 10000unibi
```

### Grab the txhash
![](https://github.com/toolfun/_pics/raw/main/nbbrdcstexmp.jpg)

### Check the recipient's balance after making a transfer
It should increase with the `amount` value entered above    
replace nibi12su8wat7ks8yvfgqls6n9eyygjlx3r6d4ex9lq with your `recepient` address. It can be any Nibiru address
```
RECIPIENT_BALANCE='{"balance": {"address": "nibi12su8wat7ks8yvfgqls6n9eyygjlx3r6d4ex9lq"}}'
```
```
nibid query wasm contract-state smart $CONTRACT "$RECIPIENT_BALANCE" --output json
```

![](https://raw.githubusercontent.com/toolfun/_pics/main/qwasmcontrstt.jpg)


____

## Done
