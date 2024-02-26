`# mainnet` `# 1.36.5`
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

### Replacing the config.json with added parameters
```
rm ~/.near/config.json
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/mainnet/config.json -P ~/.near/
```

### Install the AWS CLI
```
sudo pip3 install awscli
```

### Download all the headers and blocks - chain snapshot
```
aws s3 --no-sign-request cp s3://near-protocol-public/backups/mainnet/rpc/latest .
LATEST=$(cat latest)
aws s3 --no-sign-request cp --no-sign-request --recursive s3://near-protocol-public/backups/mainnet/rpc/$LATEST ~/.near/data
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
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

###
```
sudo systemctl daemon-reload
sudo systemctl enable --now neard && journalctl -u neard -f --no-hostname -o cat
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
