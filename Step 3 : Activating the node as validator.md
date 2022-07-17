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
