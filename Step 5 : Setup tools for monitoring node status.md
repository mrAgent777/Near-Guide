### Step 5 : Setup tools for monitoring node status
____

* __Log Files__

> Note. The log file is stored either in the ~/.nearup/logs directory or in systemd depending on your setup.
```
journalctl -n 100 -f -u neard
```
> Note. We can make log output in pretty print by installing the ccze
```
sudo apt install ccze
```

* __View Logs with color__
```
journalctl -n 100 -f -u neard | ccze -A
```
____

* __It is also recommended to use htop to view the processes by pre-installing__
```
sudo apt-get install htop
htop
```

![htop](https://user-images.githubusercontent.com/101806416/179506415-39f3db04-948d-4f4e-937c-32e72b8c3738.png)
____

### RPC
Any node within the network offers RPC services on port 3030 as long as the port is open in the nodes firewall. The NEAR-CLI uses RPC calls behind the scenes. Common uses for RPC are to check on validator stats, node version and to see delegator stake, although it can be used to interact with the blockchain, accounts and contracts overall.

Find many commands and how to use them in more detail here: https://docs.near.org/docs/api/rpc
```
sudo apt install curl jq
```

* __Check your node version__
```
curl -s http://127.0.0.1:3030/status | jq .version
```

* __Check Delegators and Stake__
```
near view <your pool>.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId <accountId>.shardnet.near
```

* __Check Reason Validator Kicked__
```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("<POOL_ID>"))' | jq .reason
```

* __Check Blocks Produced / Expected__
```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.current_validators[] | select(.account_id | contains ("POOL_ID"))'
```




