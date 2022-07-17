### Step 4 : Deploy a new staking pool for your validator
____

### Mounting a staking pool

NEAR uses a staking pool factory with a whitelisted staking contract to ensure delegators’ funds are safe. In order to run a validator on NEAR, a staking pool must be deployed to a NEAR account and integrated into a NEAR validator node. Delegators must use a UI or the command line to stake to the pool. A staking pool is a smart contract that is deployed to a NEAR account.

* __Deploy a Staking Pool__

Calls the staking pool factory, creates a new staking pool with the specified name, and deploys it to the indicated accountId.

