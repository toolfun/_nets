`# mainnet` `# 1.38.0`
# NEAR


____


### Update and install developer tools
```
sudo apt update
sudo apt install python3-pip clang install build-essential git make binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo awscli -y
```

### If python was not installed
```
sudo apt install python-is-python3
```

### Rust installation
```
sudo curl https://sh.rustup.rs -sSf | sh -s -- -y
source $HOME/.cargo/env
rustup update
```

### Clone and compile binary
```
cd
sudo rm -r $HOME/nearcore
git clone https://github.com/near/nearcore
cd nearcore
git fetch origin --tags
git checkout tags/1.36.5 -b mynode
make release
```
```
sudo mv $HOME/nearcore/target/release/neard /usr/local/bin/neard
```

### Init 
```
neard --home ~/.near init --chain-id mainnet --download-genesis --download-config
```

### Replacing the `config.json` with added parameters
```
rm ~/.near/config.json
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/mainnet/config.json -P ~/.near/
```

### Install the AWS CLI
```
sudo pip3 install awscli
```

### Download a snapshot - all the headers and blocks
> 👉 it could take many hours, use screen or tmux
```
aws s3 --no-sign-request cp s3://near-protocol-public/backups/mainnet/rpc/latest .
LATEST=$(cat latest)
aws s3 --no-sign-request cp --no-sign-request --recursive s3://near-protocol-public/backups/mainnet/rpc/$LATEST ~/.near/data
```

### An alternative method of downloading a snapshot. Using `rclone`
- #### Install rclone
```
sudo apt install rclone
```

- #### Prepare the rclone configuration
```
mkdir -p ~/.config/rclone
touch ~/.config/rclone/rclone.conf
```
 
- #### Add the following configuration in `~/.config/rclone/rclone.conf`
```
[near_s3]
type = s3
provider = AWS
location_constraint = EU
acl = public-read
server_side_encryption = AES256
region = ca-central-1
```

- #### Download the latest snapshot
> 👉 it could take many hours, use screen or tmux
```
chain="mainnet" # or "testnet"
kind="rpc" # or "archive" for Archive nodes
rclone copy --no-check-certificate near_s3://near-protocol-public/backups/${chain}/${kind}/latest ./
latest=$(cat latest)
rclone copy --no-check-certificate --progress --transfers=6 \
  near_s3://near-protocol-public/backups/${chain}/${kind}/${latest} ~/.near/data
```

### Create service
```
sudo tee /etc/systemd/system/neard.service > /dev/null << EOF
[Unit]
Description=Near_#M_node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which neard) --home $HOME/.near run
Restart=always
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

### Run the node
```
sudo systemctl daemon-reload
sudo systemctl enable --now neard && journalctl -u neard -f --no-hostname -o cat
```

###
```

```

