## Running Scripts to test Dutch Auction

In order to help run, manage, and interact with the Token and Auction contract there are a couple helper scripts in the `scripts/` directory. Once the contracts have been deployed, you can run these scripts to interact with them. ENV vars can be used to configure these scripts. NOTE: Contract addresses are deterministic, which means if you deploy the contracts for the first time to a fresh ganache instance the addresses for those contracts will be the same each time.

It is also possible to use these scripts to test contracts on Ropsten testnet by setting the `MNEMONIC` for the hdwallet and `NODE_ADDRESS` to a Ropsten testnet node.

It is **NOT** recommended to try using these scripts on the main Ethereum network as the commands will require the use of Ether for gas and have not been optimized for gas usage.

First make sure the truffle migrations have been deployed to the ganache network:

```
truffle migrate --network ganache --reset
```

Output from the deploy will show the `TOKEN_ADDRESS` and `CONTRACT_ADDRESS` to use. Example:

```
Using network 'ganache'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0xe5bb9bc190c22957b2833ed85f13d29aba71b2df695af5eb9527f8e21e5f3e19
  Migrations: 0xc6d8f6973b1579041a6f398ebe5106b3c4749ca0
Saving successful migration to network...
  ... 0x91e6d3d7a24ff9b0b53bae215d0cc7fa1bfaf472ffc78991f2595b9bb8efd817
Saving artifacts...
Running migration: 2_deploy_contract.js
  Deploying XchngToken...
  ... 0x49484698929e2081f228370a7ffb55c4f1be2084b62a9f55eaf8ffa70d587caf
  XchngToken: 0x820ed62b1dfcd1e18a9c9c83de058b28059c0ab0
Saving successful migration to network...
  ... 0xfc2a90de71955c5f111c23c48ca865fd77e06f1cb3fed7f99639dd428bcd48d2
Saving artifacts...
Running migration: 3_deploy_dutch_acution.js
  Deploying DutchAuction...
  ... 0x8b87df3f873ebf5ab3ec1e3e3252e3e05e3016b6da2a52d07236376e000b82fb
  DutchAuction: 0x89c83c691cb22c42aee0b13c3709f0d305b2a163
Saving successful migration to network...
  ... 0x16ea0e94b58dac6e047a210adcaf33d441fe8c28fe52c5724178d06dcae0724c
Saving artifacts...
```

`TOKEN_ADDRESS = 0x820ed62b1dfcd1e18a9c9c83de058b28059c0ab0`

`CONTRACT_ADDRESS = 0x89c83c691cb22c42aee0b13c3709f0d305b2a163`

### Setup

The setup script will initialize the Xchng Token, authorize the Auction to transfer tokens, setup the auction, and start it. To run the setup script simply run the following command.

```
node scripts/setup.js
```

#### Configuration

| ENV | Default | Description |
|-----|---------|-------------|
| NODE_ADDR | http://localhost:7545 | By default set to local ganache |
| MNEMONIC | none | Mnemonic used to generate HDWallet accounts |
| AUCTION_ADDRESS | none | The address of the auction |
| TOKEN_ADDRESS | none | The address of the token |
| NUM_TOKENS_AVAILABLE | '600000000000000000000000000' | The number of Xchng tokens to preallocate |

### Bid

The bid script will submit a bid to the auction. By default it bids 1 ETH, and chooses a random ganache account to bid from. Both of those behaviors can be overridden.

```
node scripts/bid.js
```

#### Configuration

| ENV | Default | Description |
|-----|---------|-------------|
| NODE_ADDR | http://localhost:7545 | By default set to local ganache |
| MNEMONIC | none | Mnemonic used to generate HDWallet accounts |
| AUCTION_ADDRESS | none | The address of the auction |
| TOKEN_ADDRESS | none | The address of the token |
| FROM_ADDRESS | None | The address to bid from, by default this is 'None' which causes the script to randomly choose one of the ganache addresses to bid from |
| BID_AMOUNT | '1' | The amount of ETH to bid, this needs to be _at least_ 1 ETH |

### Pause

The pause script will pause either the token or auction contract.  The owner of the contracts must be the account[0] generated by the HDWallet mnemonic.

```
node scripts/pause.js [pause | unpause] [token | auction]
```

#### Configuration

| ENV | Default | Description |
|-----|---------|-------------|
| NODE_ADDR | http://localhost:7545 | By default set to local ganache |
| MNEMONIC | none | Mnemonic used to generate HDWallet accounts |
| AUCTION_ADDRESS | none | The address of the auction |
| TOKEN_ADDRESS | none | The address of the token |

### Whitelist

The whitelist script will whitelist an address for the Dutch Auction contract.  The owner of the auction contract must be the account[0] generated by the HDWallet mnemonic.

```
node scripts/whitelist.js [Address]
```

#### Configuration

| ENV | Default | Description |
|-----|---------|-------------|
| NODE_ADDR | http://localhost:7545 | By default set to local ganache |
| MNEMONIC | none | Mnemonic used to generate HDWallet accounts |
| AUCTION_ADDRESS | none | The address of the auction |