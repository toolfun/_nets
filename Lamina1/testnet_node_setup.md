# 🚧 outdated
### Lamina1. Node setup giude.
____

### Links
- Testnet Hub: wallet, faucet, docs https://testnet.lamina1.network/
- Crew3: https://crew3.xyz/c/lamina1/questboard

### System requirements (taken from official docs)
- CPU: Equivalent of 2 AWS vCPU
- RAM: 4 GiB
- Storage: 100 GiB
- OS: Ubuntu 22.04 or MacOS >= Catalina

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
wget https://lamina1.github.io/lamina1/lamina1.latest.ubuntu-latest.tar.gz
tar -xvzf lamina1.latest.ubuntu-latest.tar.gz
```

### Download config
```
cd $HOME/lamina1
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
> #### Detach screen
> ```
> Ctrl+A, D
> ```

<!-- FOR SERVICE FILE
### Open config file and edit as shown
```
nano $HOME/lamina1/configs/testnet/default.json
```

  `"data-dir": "$HOME/.lamina1/238426347/default",`
  `"genesis": "$HOME/lamina1/configs/testnet/Genesis.json",`

### Create service file
```
sudo tee /etc/systemd/system/lamina1d.service > /dev/null <<EOF
[Unit]
Description=Lamina1 Node
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/lamina1/lamina1-node  --config-file $HOME/lamina1/configs/testnet/default.json
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
```
systemctl daemon-reload
systemctl enable lamina1d
systemctl restart lamina1d && journalctl -u lamina1d -f -o cat
```
-->

### Bootstrap status for each chain
```
bash $HOME/lamina1/check-bootstrap.sh
```
> The output is *"true"* if bootstrapped: **Your node is running and connected now.**    
> The output is *"API call rejected"* if not.


### If you want to add your node as a validator
- #### Generate a wallet address https://wallet-test.lamina1.network
- #### 2000 tokens.
> Using the Web Wallet, check to make sure you have at least **2000** L1 tokens on the **P-Chain**. You can
use the "cross chain" function to transfer your C-Chain or X-Chain tokens to the P chain. If you’d like to
get enough testnet tokens to run a node, please contact the team on Discord.    
- #### Get your node-id
> ```bash
> cd $HOME/lamina1/ && ./get_my_nodeid.sh; cd 
> ```
- #### Use the Web Wallet to stake your tokens on that node ID, under the "Earn" option.

#
### Delete lamina1 node
#### Terminate lamina1 screen
> #### Attach screen
> ```
> screen -dr lamina1
> ```
> #### Type `exit` **in a screen**
> ```
> exit
> ```
#### Remove Lamina1 directories
> ```
> rm -r $HOME/lamina1 $HOME/.lamina1
> ```

#

### Usefull commands
### Create screen
```bash
# Create screen with a name lamina1
screen -S lamina1
```

### Detach from the screen
> Ctrl-A, D

### Attach to a screen lamina1
```
screen -r lamina1
```

### Exit (terminate) screen
```bash
# in the (!)attached screen type <exit>
exit
```

### Check logs
```
tail -f -n 50 ~/.lamina1/238426347/default/logs/C.log
```

____



