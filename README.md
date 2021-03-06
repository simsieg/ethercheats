# ethercheats

## Cheatsheet for Private Ethereum Chains

### Installation

Install `geth` from https://geth.ethereum.org/downloads/.
In Windows: Add installation path to environment variable `PATH`.

Create a folder for your Blockchain: `blockChainFolder`

### Blockchain Creation

Create a `genesis.json` in your `blockChainFolder`.
Use a random `chainId`, keep `difficulty` low to mine fast.

```json
{
    "config": {
        "chainId": 2017,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "0x10000",
    "gasLimit": "2100000",
    "alloc": {}
}
```

### Blockchain Setup

You can either create a private chain or connect to a network:

#### Private Chain

Create a private blockchain in your `blockChainFolder` with your shell:

```sh
geth --datadir="ethdata" init genesis.json
```

Create a network for the private blockchain and start a console, chose a random (high) networkid:

```sh
geth --datadir="ethdata" --networkid 20123 console
```

Now you have access to your blockchain via console.

#### Connect to Network

Connect to a network:

```shell
geth --datadir="ethdata" --networkid [aNetworkId] --rpcport [aPort] --rpcaddr [aIp] --rpc console --ipcdisable
```

`networkid` - the networkid of your chain e.g. 20123 

`rpcport` - a unique port e.g. 8001

`rpcaddr` - your address in the network e.g. 10.0.0.2


Get your enode
```javascript
admin.nodeInfo.enode
```
This looks like

`enode://616f524fecc375df460c7d47fc84d9b2049a1feac0d15c956a7b93ae84916e2fbd928b2d544d16c5f9be8d5516992019438bcdebaba7177248c624fc2bd34aac@[::]:30303`

Replace `[::]` with your ip and give it to your partner.

Your partner adds you with:

```
admin.addPeer('enode://616f524fecc375df460c7d47fc84d9b2049a1feac0d15c956a7b93ae84916e2fbd928b2d544d16c5f9be8d5516992019438bcdebaba7177248c624fc2bd34aac@10.0.0.2:30303')
```

See all participants with

```
admin.peers
```

### Accounts and Balances

Create an account:

```javascript
personal.newAccount('insertYourPasswordHere')
```

You receive an address like `0x29fbf84ff0f76d0b720d1b22de0aabd8779e362d`.

Show all accounts:

```javascript
eth.accounts
```

Assuming yours is the first in the list, you get your Balance with:

```javascript
eth.getBalance(eth.accounts[0])
```

You can also pass your address directly to the `getBalance` function.

```javascript
eth.getBalance('0x29fbf84ff0f76d0b720d1b22de0aabd8779e362d')
```

### Mining

To get ether, we need to mine some blocks.

Bind the miner to your address:
```javascript
miner.setEtherbase(eth.accounts[0])
```

To start mining:
```javascript
miner.start()
```

Wait some time, then stop the miner with:
```javascript
miner.stop()
```

Let's take a look how rich you are:

```javascript
eth.getBalance(eth.accounts[0])
```

### Transactions

Now create another account like explained above.

Unlock your account:

```javascript
personal.unlockAccount(eth.accounts[0], 'insertYourPasswordHere')
```

It's time to spend your money:

```javascript
eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: 202020})
```

The money is not directly available in the second account, the transaction has to be mined first.
So start and stop the miner as explained above and check out the balances.



