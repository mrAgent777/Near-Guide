### Step 2 : Setup a validator and sync it to the actual state of the network
____

* __Before installation, let's check if our equipment is suitable:__
```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
  ```
  > In my case everything is fine, so let's move on
  >> ![V1](https://user-images.githubusercontent.com/101806416/179396253-2e2d966b-174f-45e9-af74-c99c7238d9f6.png)

* __First, let's update linux__
```
sudo apt update && sudo apt upgrade -y
```

* __Now Install developer tools, Node.js, and npm__
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
sudo apt install build-essential nodejs
PATH="$PATH"
```

* __Let's check the version Node.js__
``` 
node -v
```
> ![v2](https://user-images.githubusercontent.com/101806416/179396939-271d69c7-59f0-4fd0-8fd7-8c03c8a1403c.png)


* __Let's check the version Npm__
``` 
npm -v
```
> ![v3](https://user-images.githubusercontent.com/101806416/179396940-3bffcb6e-3a20-4a64-b2bf-e08cbbfc4d30.png)
___
### Install NEAR-CLI

Here's the Github Repository for NEAR CLI.: https://github.com/near/near-cli. NEAR-CLI is a command-line interface that communicates with the NEAR blockchain via remote procedure calls (RPC)
```
sudo npm install -g near-cli
```

* __Setup Shardnet (this is the network we will use for Stake Wars)__
```
export NEAR_ENV=shardnet
```
> Note. You can also run this command to set the Near testnet Environment persistent:
```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
```
___
### Validator Stats

Now that NEAR-CLI is installed, let's test out the CLI and use the following commands to interact with the blockchain as well as to view validator stats.

> * __Proposals:__ 
>> * A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.
```
near proposals
```

> * __Validators Current:__
>> * This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.
```
near validators current
```

> * __Validators Next:__
>> * This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.
```
near validators next
```
____

### Setup your node

Now begin to smoothly install the necessary software and configurations

* __Install developer tools__

```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```

* __Install Python pip__
```
sudo apt install python3-pip
```

* __Set the configuration__
```
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```

* __Install Building env__
```
sudo apt install clang build-essential make
```

* __Install Rust & Cargo__
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
> Note. Press 1 and press enter
>> ![v4](https://user-images.githubusercontent.com/101806416/179398919-2dafd8db-2ffc-4a92-aad5-d3fce0f7254d.png)

* __Source the environment__
```
source $HOME/.cargo/env
```
____
### Clone and build nearcore project from GitHub
* __First, clone the nearcore__
```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```

* __Checkout master__
```
git checkout master
```

* __Compile nearcore binary__
```
cargo build -p neard --release --features shardnet
```

> Note. Compiling nearcore binary may take a little while.
>> ![v5](https://user-images.githubusercontent.com/101806416/179401701-bb7b37e9-b7f3-4761-89cc-2ccd4d8e91c7.png)

### Initialize working directory
In order to work properly, the NEAR node requires a working directory and a couple of configuration files. Generate the initial required working directory
```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```
#### This command will create the directory structure and will generate `config.json`, `node_key.json`, and `genesis.json` on the network you have passed.

> ![v6](https://user-images.githubusercontent.com/101806416/179401891-8c47950c-f6bf-4500-bb4f-97d5eb1d09e0.png)

* __config.json__ - Configuration parameters which are responsive for how the node will work. 

* __genesis.json__ - A file with all the data the network started with at genesis. 

* __node_key.json__ - A file which contains a public and private key for the node. Also includes an optional account_id parameter which is required to run a validator node (not covered in this doc).

* __data/__ - A folder in which a NEAR node will write it's state.
____

### Replace the config.json
```
cd ~/.near
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```
____
### Synchronizing the network with the last snapshot
* __Install AWS Cli__
```
cd ..
sudo apt-get install awscli -y
```

* __Download snapshot__
```
cd ~/.near
aws s3 --no-sign-request cp s3://build.openshards.io/stakewars/shardnet_noarchive/data.tar.gz .  
tar -xzvf data.tar.gz
```
____
### Run the node
* __To start your node simply run the following__
```
cd ..
cd nearcore
./target/release/neard --home ~/.near run
```
> Note. The node is now running you can see log outputs in your console. Your node should be find peers, download headers to 100%, and then download blocks.
>> ![v7](https://user-images.githubusercontent.com/101806416/179403606-a219dfa9-3e4b-4106-a995-6f9c0f437f56.png)






