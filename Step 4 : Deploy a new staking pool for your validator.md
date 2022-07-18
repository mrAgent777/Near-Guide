### Step 4 : Deploy a new staking pool for your validator
____

### Mounting a staking pool

NEAR uses a staking pool factory with a whitelisted staking contract to ensure delegators’ funds are safe. In order to run a validator on NEAR, a staking pool must be deployed to a NEAR account and integrated into a NEAR validator node. Delegators must use a UI or the command line to stake to the pool. A staking pool is a smart contract that is deployed to a NEAR account.

* __Deploy a Staking Pool__

Calls the staking pool factory, creates a new staking pool with the specified name, and deploys it to the indicated accountId.
```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "secdec", "owner_id": "secdec.shardnet.near", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="secdec.shardnet.near" --amount=30 --gas=300000000000000
```

> Note. From the example above, you need to replace:

* __Change `secdec` for your pool id__

* __Change `secdec.shardnet.near` for your wallet__

* __Change `Public key` for your `validator_key.json` file__

> Note. If there is a “True” at the End. Your pool is created. Check your pool is now visible on https://explorer.shardnet.near.org/nodes/validators

* __To change the pool parameters, such as changing the amount of commission charged to 1%__
```
near call secdec.factory.shardnet.near update_reward_fee_fraction '{"reward_fee_fraction": {"numerator": 1, "denominator": 100}}' --accountId secdec.shardnet.near --gas=300000000000000
```

____
### Transactions Guide

> Note. Don't forget to change the values to your own

* __Deposit and Stake NEAR__
```
near call <staking_pool_id> deposit_and_stake --amount <amount> --accountId <accountId> --gas=300000000000000
```

* __Unstake NEAR__

> Note. Amount in yoctoNEAR.
```
near call <staking_pool_id> unstake '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```

* __To unstake all__
```
near call <staking_pool_id> unstake_all --accountId <accountId> --gas=300000000000000
```

* __Withdraw__

> Note. Unstaking takes 2-3 epochs to complete, after that period you can withdraw in YoctoNEAR from pool.
```
near call <staking_pool_id> withdraw '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```

* __withdraw all__
```
near call <staking_pool_id> withdraw_all --accountId <accountId> --gas=300000000000000
```

* __Ping__

> Note. A ping issues a new proposal and updates the staking balances for your delegators. A ping should be issued each epoch to keep reported rewards current.
```
near call <staking_pool_id> ping '{}' --accountId <accountId> --gas=300000000000000
```
____
### Balances

* __Total Balance__
```
near view <staking_pool_id> get_account_total_balance '{"account_id": "<accountId>"}'
```

* __Staked Balance__
```
near view <staking_pool_id> get_account_staked_balance '{"account_id": "<accountId>"}'
```

* __Unstaked Balance__
```
near view <staking_pool_id> get_account_unstaked_balance '{"account_id": "<accountId>"}'
```

* __Available for Withdrawal__

> Note. You can only withdraw funds from a contract if they are unlocked.
```
near view <staking_pool_id> is_account_unstaked_balance_available '{"account_id": "<accountId>"}'
```
____
### Pause / Resume Staking

* __Pause__
```
near call <staking_pool_id> pause_staking '{}' --accountId <accountId>
```

* __Resume__
```
near call <staking_pool_id> resume_staking '{}' --accountId <accountId>
```
____
## [Step 5 : Setup tools for monitoring node status.](https://github.com/mrAgent777/Near-Guide/blob/main/Step%205%20:%20Setup%20tools%20for%20monitoring%20node%20status.md)







