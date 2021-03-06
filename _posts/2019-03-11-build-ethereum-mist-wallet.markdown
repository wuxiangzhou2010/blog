---
layout: post
title: "Build mist wallet version 0.10.0 "
date: 2019-03-11 16:52 +0800
categories: Ethereum
published: true
---

Mist `0.10.0` it is the last version that use web3.js `0.xx.xx` series. To not break dependencies in my other projects, I need to use this version rather the newer ones.

As far as I know, the major difference between web3.js 0.xx and 1.xx is that 1.xx is mostly using async mechanism and web3 is divided into several packages while 0.xx can use async and sync mechanism and 0.xx web3.js is packed into a single package.

### Config foleder

linux: `~/.config/Mist`

```sh
rm -rf ~/.config/Mist
```

### Dependencies

- nodejs 8
- meteor
- yarn
- Electron
- Gulp

```sh
nvm install v8.14.1
curl https://install.meteor.com/ | sh
curl -o- -L https://yarnpkg.com/install.sh | bash
yarn global add electron@1.8.4
yarn global add gulp
```

you may also need to install

```sh
sudo apt update && sudo apt install curl git python-dev build-essential -y
```

### Get repo

```sh
git clone https://github.com/ethereum/mist.git

cd mist
git reset --hard v0.10.0
yarn
```

### For dev

```sh
# 1. first terminal
cd mist/interface && meteor --no-release-check
#may need to install below too
meteor npm install --save babel-runtime

# 2. second terminal
git clone https://github.com/ethereum/meteor-dapp-wallet.git
cd meteor-dapp-wallet
# this is the last web3.js 0.xx version, on Feb 23, 2018, Begin upgrading to web3.js 1.0.0
git reset --hard  f8ea87c
cd app && meteor --port 3050
# npm install @babel/runtime@7.0.0-beta.49

# 3. third terminal
cd mist
# ipc
yarn dev:electron --mode wallet --rpc /home/username/.ethereum/geth.ipc
# http
yarn dev:electron --mode wallet --rpc http://172.16.3.191:8545
```

### Generate wallet

Build for windows on Linux

- [install-wine-on-ubuntu-18-04-bionic-beaver-linux](https://linuxconfig.org/install-wine-on-ubuntu-18-04-bionic-beaver-linux)

```sh
sudo dpkg --add-architecture i386
sudo apt install wine64
sudo apt install wine32
wine --version
gulp --win --wallet
```

```sh
# install multi lib for ia32 platform
sudo apt-get install gcc-multilib g++-multilib

npm install -g meteor-build-client
gulp --linux --wallet
```

Reference:

- [ethereum mist v0.10.0](https://github.com/ethereum/mist/tree/v0.10.0)
