### Lamina1. Validator setup
> #### You'll need Ubuntu 22.
### 🚧🚧🚧 under construction

#
### Links
- #### Testnet Hub: wallet, faucet, docs https://testnet.lamina1.network/
- #### Crew3: https://crew3.xyz/c/lamina1/questboard
#

### Update && upgrade
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl tar wget clang pkg-config libssl-dev build-essential jq git make ncdu gcc chrony screen htop ncdu -y
```

### Download the latest release 
```
https://lamina1.github.io/lamina1/lamina1.latest.ubuntu-latest.tar.gz
tar -xvzf lamina1.latest.ubuntu-latest.tar.gz
```

> We have lamina1 directory

### Download config
```
cd lamina1
curl https://lamina1.github.io/lamina1/config.testnet.tar | tar xf -
```

### We will run the lamina1-node in a screen
```
sudo apt update && sudo apt install screen -y &&\
screen -S lamina1
```
```
cd $HOME/lamina1 && ./lamina1-node  --config-file configs/testnet/default.json
```

### Checking logs about node bootstrapping for each chain
```
cd $HOME/lamina1 && ./check-bootstrap.sh; cd
```
> The output is *"true"* if bootstrapped: **Your node is running and connected now.**    
> The output is *"API call rejected"* if not.


### If you want to add your node as a validator
- #### Generate a wallet address https://wallet-test.lamina1.network
- #### 2000 tokens.
> Using the Web Wallet, check to make sure you have at least **2000** L1 tokens on the **P-Chain**. You can
use the "cross chain" function to transfer your C-Chain or X-Chain tokens to the P chain. If you’d like to
get enough testnet tokens to run a node, please contact the team on Discord.    
- Get your node-id
> ```bash
> cd $HOME/lamina1/ && ./get_my_nodeid.sh; cd 
> ```
- Use the Web Wallet to stake your tokens on that node ID, under the "Earn" option from the dropdown
menu on the upper right











#
## Usefull commands
### Create screen
```bash
# Create screen with a name lamina1
screen -S lamina1
```

### Detach from the screen
> Ctrl-A, D

### Attach to a screen *lamina1*
```
screen -r lamina1
```

### Exit (terminate) screen
```bash
# in the attached screen type <exit>
exit
```







