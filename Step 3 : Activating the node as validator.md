### Step 3 : Activating the node as validator
____

#### __Authorize Wallet Locally__

A full access key needs to be installed locally to be able to sign transactions via NEAR-CLI.
```
near login
```
> Note: This command launches a web browser allowing for the authorization of a full access key to be copied locally.
> Copy the link and paste it into your browser
>> ![w1](https://user-images.githubusercontent.com/101806416/179422127-b9873b57-4018-49ee-9cf1-07a0843556eb.png)

> Allow access for Near CLI
>> ![w2](https://user-images.githubusercontent.com/101806416/179422128-1050cf36-724e-4532-b555-2e861d5d9fdd.png)

> After success you will see a page like this, copy your wallet address and return to the console
>> ![w3](https://user-images.githubusercontent.com/101806416/179422129-28090d63-6458-4e65-bde7-95b2e068d90f.png)

> Enter your wallet and press Enter
>> ![w4](https://user-images.githubusercontent.com/101806416/179422131-6a5d6e8c-7b71-4ce3-8ad3-28cc113c5902.png)

#### __Create a validator_key.json__

* __Generate the Key file:__
> Note. Make sure to replace `secdec` for your pool id.
```
near generate-key secdec.factory.shardnet.near
```

* __Copy the file generated to shardnet folder:__ 
> Note. Make sure to replace `secdec` by your wallet
```
cp ~/.near-credentials/shardnet/secdec.shardnet.near.json ~/.near/validator_key.json
```

* __The content of the file validator_key.json needs to be changed slightly:__
```
nano ~/.near/validator_key.json
```

> Note. The account_id must match the staking pool contract name or you will not be able to sign blocks.

Edit “account_id” => secdec.factory.shardnet.near, where secdec is your PoolName

Change `private_key` to `secret_key`

> It's been
```
{
  "account_id": "secdec.shardnet.near",
  "public_key": "ed25519:HeaBJ3xLgvZacQWmEctTeUqyfSU4SDEnEwckWxd92W2G",
  "private_key": "ed25519:****"
}
```

> This has become
```
{
  "account_id": "secdec.factory.shardnet.near",
  "public_key": "ed25519:HeaBJ3xLgvZacQWmEctTeUqyfSU4SDEnEwckWxd92W2G",
  "secret_key": "ed25519:****"
}
```
> __Save__ `Ctrl+o`  `Enter`  `Ctrl+x`
____
* __Start the validator node__
```
cd nearcore
target/release/neard run
```

* __Setup Systemd Command__
```
sudo nano /etc/systemd/system/neard.service
```
Paste 
> Note. Change `root` to your paths
```
[Unit]
Description=NEARd Daemon Service

[Service]
Type=simple
User=root
#Group=near
WorkingDirectory=/root/.near
ExecStart=/root/nearcore/target/release/neard run
Restart=on-failure
RestartSec=30
KillSignal=SIGINT
TimeoutStopSec=45
KillMode=mixed

[Install]
WantedBy=multi-user.target
```
> __Save__ `Ctrl+o`  `Enter`  `Ctrl+x`

 * __Enable service__
 ```
sudo systemctl enable neard
```

* __Start service__
```
sudo systemctl start neard
```

* __If you need to make a change to service because of an error in the file. It has to be reloaded:__
```
sudo systemctl reload neard
```

* __Check logs__
```
journalctl -n 100 -f -u neard
```
____
### Becoming a Validator

In order to become a validator and enter the validator set, a minimum set of success criteria must be met.

* The node must be fully synced
* The `validator_key.json` must be in place
* The contract must be initialized with the public_key in `validator_key.json`
* The account_id must be set to the staking pool contract id
* There must be enough delegations to meet the minimum seat price. See the seat price [here.](https://explorer.shardnet.near.org/nodes/validators)
* A proposal must be submitted by pinging the contract
* Once a proposal is accepted a validator must wait 2-3 epoch to enter the validator set
* Once in the validator set the validator must produce great than 90% of assigned blocks

Check running status of validator node. If “Validator” is showing up, your pool is selected in the current validators list.
___

## Moving on [Step 4 : Deploy a new staking pool for your validator](https://github.com/mrAgent777/Near-Guide/blob/main/Step%204%20:%20Deploy%20a%20new%20staking%20pool%20for%20your%20validator.md)


