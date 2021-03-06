# Deploying to Testnet

Following the steps provided by Truffle to deploy to a live network.
http://truffleframework.com/tutorials/deploying-to-the-live-network

## Setting up Ethereum Client

Using [go-ethereum](https://github.com/ethereum/go-ethereum) to setup an Ethereum client connected to the [Ropsten](https://github.com/ethereum/ropsten) testnet.

Install Geth for testnet

```
geth --testnet
```

Sync geth client with network

```
geth --testnet --fast --bootnodes "enode://20c9ad97c081d63397d7b685a412227a40e23c8bdc6688c6f37e97cfbc22d2b4d1db1510d8f61e6a8866ad7f0e17c02b14182d37ea7c3c8b9c2683aeb6b733a1@52.169.14.227:30303,enode://6ce05930c72abc632c58e2e4324f7c7ea478cec0ed4fa2528982cf34483094e9cbc9216e7aa349691242576d552a2a56aaeae426c5303ded677ce455ba1acd9d@13.84.180.240:30303"
```

## Configuring Truffle

Updating the truffle.js file to setup the Ropsten network configuration.

This uses the [HD Wallet Provider](https://github.com/trufflesuite/truffle-hdwallet-provider) to set the provider with the mnemonic and address set as environment variables.

```
const HDWalletProvider = require("truffle-hdwallet-provider");

const mnemonic = process.env.MNEMONIC;
const node = process.env.NODE_ADDR;

module.exports = {
  networks: {
    ganache: {
      host: "127.0.0.1",
      port: 7545,
      network_id: "5777"
    },
    ropsten: {
      provider: function () {
        return new HDWalletProvider(mnemonic, node)
      },
      network_id: 3,
      gas: 4700000
    }
  }
};
```

## Deploying to the Ropsten Network

Run the following command to deploy:

```
truffle migrate --network ropsten --verbose-rpc
```

Verbose rpc is optional, but provides feedback on the rpc connection in case there is an issue that needs debugged.

Output should look like:

```
using network 'ropsten'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0xe5bb9bc190c22957b2833ed85f13d29aba71b2df695af5eb9527f8e21e5f3e19
  Migrations: 0x42e59ca6deeeb78dd25dd2ca86848fa821f5d8a6
Saving successful migration to network...
  ... 0x611f1aa2fe437a3257943f0bf607a7de0e1cff4a93ba8339c26bdb528bc63883
Saving artifacts...
Running migration: 2_deploy_contract.js
  Deploying XchngToken...
  ... 0xe0d49d5bb4b1cb6c744f844f819b97f12db8b4c9f170ceec786684f766a32d35
  XchngToken: 0x9e95e55decc28643126255855196dd85a53adcf3
Saving artifacts...
```

## When going to Live Mainnet

Add the following network to the truffle.js file:

```
"live": {
  provider: function () {
    return new HDWalletProvider(mnemonic, node)
  },
  network_id: 1, // live Ethereum network
  gas: 4700000
}
```

Run the migrate command with live network specified:

```
truffle migrate --network live
```