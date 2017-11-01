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

Now either we can create a private blockchain or connect to a network:

#### Private chain

Create a private blockchain in your `blockChainFolder` with your shell:

```sh
geth --datadir="ethdata" init genesis.json
```

Create a network for the private blockchain and start a console, chose a random (high) networkid:

```sh
geth --datadir="ethdata" --networkid 20123 console
```

Now you have access to your blockchain via console.

#### Network

```javascript
geth --datadir ethdata --networkid [aNetworkId] --rpcport [aPort] --rpcaddr [anIp] --rpc console --ipcdisable
```

`networkid` has to be the same for all participants, e.g. `20123`

`rpcport` has to be unique for each participant, e.g. `8002`

`rpcaddr` is your address in the network, e.g. `10.0.0.2`


Get your `enode` id:

```javascript
admin.nodeInfo.enode
```

Example result:

`enode://a0847a397bc504480b9991787db756502d5c723a2f7f3e0e75dc03849e0ee6d374428f95b907b77020a4308986bdd59c4d091b2d9c523e4fa2eb654de448ec02@[::]:30303`

Replace `[::]` with your rpcaddr and send it to your blockchain partner, e.g.

`enode://a0847a397bc504480b9991787db756502d5c723a2f7f3e0e75dc03849e0ee6d374428f95b907b77020a4308986bdd59c4d091b2d9c523e4fa2eb654de448ec02@10.0.0.2:30303`.

The partner executes in his ethereum console:
```javascript
admin.addPeer('enode://a0847a397bc504480b9991787db756502d5c723a2f7f3e0e75dc03849e0ee6d374428f95b907b77020a4308986bdd59c4d091b2d9c523e4fa2eb654de448ec02@10.0.0.2:30303')
```

With `admin.peers` one can see all the participants of the network.

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

