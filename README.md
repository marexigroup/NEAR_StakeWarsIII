# NEAR_StakeWarsIII

## First Challenge
Create your Shardnet wallet & deploy the NEAR CLI. 

### Create a wallet
Wallet: https://wallet.shardnet.near.org
![](https://www.oceanblock.co/wp-content/uploads/2022/07/b4f-wallet.jpg)

### Setup NEAR-CLI
Update OS:
	> sudo apt update && sudo apt upgrade -y

Install *nodejs* and *npm*:
	> curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
	> sudo apt install build-essential nodejs
	> PATH="$PATH"

Check versión:
	> near@near:~$ node -v; npm -v;
	v18.6.0
	8.13.2

Install NEAR-CLI
	> sudo npm install -g near-cli

### Validator Stats
	 export NEAR_ENV=shardnet
	 echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
	 near proposals
	 near validators current
	 near validators next
	 near validators next
	Next validators (total: 271, seat price: 200):
	.-------------------------------------------------------------------------------------------.
	|   Status   |                  Validator                  |        Stake         | # Seats |
	|------------|---------------------------------------------|----------------------|---------|
	| New        | nearfans.factory.shardnet.near              | 28,930               | 1       |
	| New        | pangdao.factory.shardnet.near               | 26,380               | 1       |
	| New        | warrior.factory.shardnet.near               | 9,430                | 1       |
	

## Second Challenge
This challenge is focused on deploying a node *(nearcore)*

### Setup machine
Check hardware
4 CPU, 8 RAM, 500G SSD. 
	 lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null && echo "Supported" || echo "Not supported"
	Supported

### Install developer tools
	sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
	
	sudo apt install python3-pip
	
	USER_BASE_BIN=$(python3 -m site --user-base)/bin
	
	export PATH="$USER_BASE_BIN:$PATH"
	
	sudo apt install clang build-essential make
	
	curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
	  stable-x86_64-unknown-linux-gnu installed - rustc 1.62.1 
	
	(e092d0b6b 2022-07-16)
	Rust is installed now. Great!


### Build nearcore
	source $HOME/.cargo/env
	
	git clone https://github.com/near/nearcore
	
	cd nearcore
	
	git fetch
	
	git checkout 0f81dca95a55f975b6e54fe6f311a71792e21698
	
	cargo build -p neard --release --features shardnet
	near@near:~/nearcore$ cargo build -p neard --release --features shardnet
	info: syncing channel updates for '1.62.0-x86_64-unknown-linux-gnu'
	info: latest update on 2022-06-30, rust version 1.62.0 (a8314ef7d 2022-06-27)
	   Compiling nearcore v0.0.0 (/home/near/nearcore/nearcore)
	   Compiling state-viewer v0.0.0 (/home/near/nearcore/tools/state-viewer)
	    Finished release [optimized] target(s) in 8m 13s

### Init NEAR directories and replace config.json
	./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
	
	rm ~/.near/config.json
	
	wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
	./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
	2022-07-22T18:06:02.400151Z  INFO neard: version="trunk" build="crates-0.14.0-234-g0f81dca95" latest_protocol=100
	2022-07-22T18:06:02.400379Z  INFO near: Using key ed25519:Be7uPPW921Xd7Y8muUvWQ6SUV9yUpLNFQhk4qhGymWVF for node
	2022-07-22T18:06:02.400416Z  INFO near: Downloading genesis file from: https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json.xz ...
	  [00:00:01] [################################################################################################################################################] 3.29MB/3.29MB [1.88MB/s] (0s)
	2022-07-22T18:06:04.947111Z  INFO near: Saved the genesis file to: /home/near/.near/genesis.json ...
... (164 lines left)
