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

### Install

```
cd $HOME
git clone https://github.com/haqq-network/haqq
cd haqq
git checkout v1.3.0
make install 
```


### Check version
```bash
haqqd version --long | head
# version: '"v1.3.0"'
# commit: 39b4dcb706e72c187fc23ec6dddcfed20c79bc9d
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