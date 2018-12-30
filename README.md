# private-blockchain
private-blockchain-lab
-- 
Developed in Mac

Use homebrew for a straight-forward installation:

``` bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Required software:
- ethereum-wallet
- geth
- solidity

``` bash
# update brew formulas
brew update
brew upgrade

# Install wallet
brew cask install ethereum-wallet

# Add Ethereum taps (third-party repositories)
brew tap ethereum/ethereum

# Install Geth
brew install ethereum

# Install Solidity
brew install solidity
```

For othe OS just look into installation guides for each tool

# Setup (on Bash)
- `geth --identity "jriveraiTestNode" --nodiscover --networkid 1999 --datadir $PROJECT_DIR/data init genesis.json`

## List accounts
- `geth account list --datadir $PROJECT_DIR/data`

## Create an account
- `geth  --datadir $PROJECT_DIR/data account new`

## Start
- `geth --identity "jriveraTestNode" --nodiscover --networkid 1999 --datadir $PROJECT_DIR/data --port "35555"`

## Attach
- `geth attach $PROJECT_DIR/data/geth.ipc`

## RemoveDB
- `geth removedb --datadir $PROJECT_DIR/data`

## Project accounts
### Account #0: {c5b68fe98eeec2d5820129b1f0f0560a3d41b424} 
- passphrase: caracoles
### Account #1: {3419b1faabc867b052483f60de73c78e3dcfd86e}
- passphrase: loveCoffee

# Geth: A JS console
- web3.eth.accounts: Return a list with available accounts
- web3.eth.getBalance("[Account-hash]"): Return the balance of given account

# Do transactions (on Geth)
*Remember: Geth is a JS console*

```
# Declare recipient account
> var testAccount = "0x3419b1faabc867b052483f60de73c78e3dcfd86e"
undefined

# Start mining
> miner.start()
null

# Unlock the house (use base account pass)
> personal.unlockAccount(web3.eth.coinbase)
Unlock account 0xc5b68fe98eeec2d5820129b1f0f0560a3d41b424
Passphrase: 
true

# Do a transaction
> web3.eth.sendTransaction({from: web3.eth.coinbase, to: testAccount, value: web3.toWei("0.1", "ether")})
"0x254f992ea005e8ef3bfaceefb3c5ca0cc9834b2706d500aac79426a53bd91dcc"

# Check transaction was completed
> web3.eth.getBalance(testAccount)
100000000000000000

# Unlock recipient account
> personal.unlockAccount(testAccount)
Unlock account 0x3419b1faabc867b052483f60de73c78e3dcfd86e
Passphrase: 
true
```

# Smart contract
## Compile (on bash)
- `solc --bin --abi SimpleStorage.sol`

Example output:

```
======= SimpleStorage.sol:SimpleStorage =======
Binary: 
608060405234801561001057600080fd5b5060da8061001f6000396000f3fe6080604052348015600f57600080fd5b5060043610604f576000357c01000000000000000000000000000000000000000000000000000000009004806360fe47b11460545780636d4ce63c14607f575b600080fd5b607d60048036036020811015606857600080fd5b8101908080359060200190929190505050609b565b005b608560a5565b6040518082815260200191505060405180910390f35b8060008190555050565b6000805490509056fea165627a7a72305820601339ee69bb90e3b52f77ffaea9c903449afb264cc499f49e260b7cc8bba4fc0029
Contract JSON ABI 
[{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}]

```

## Declare (on Geth)
```
# Make recipient the default account
> web3.eth.defaultAccount = testAccount
"0x3419b1faabc867b052483f60de73c78e3dcfd86e"

# Set Contract JSON ABI in a var
> var abi = [{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}]
undefined

# Create and save a contract
> var myContract = web3.eth.contract(abi)
undefined

# Set contract data
> var contractData = myContract.new.getData({data: "0x608060405234801561001057600080fd5b5060da8061001f6000396000f3fe6080604052348015600f57600080fd5b5060043610604f576000357c01000000000000000000000000000000000000000000000000000000009004806360fe47b11460545780636d4ce63c14607f575b600080fd5b607d60048036036020811015606857600080fd5b8101908080359060200190929190505050609b565b005b608560a5565b6040518082815260200191505060405180910390f35b8060008190555050565b6000805490509056fea165627a7a72305820601339ee69bb90e3b52f77ffaea9c903449afb264cc499f49e260b7cc8bba4fc0029"})
undefined

# Remember, you need to unlock your account to do a transaction
> personal.unlockAccount(testAccount)
Unlock account 0x3419b1faabc867b052483f60de73c78e3dcfd86e
Passphrase: 
true

# Create a contract instance
> var contractInstance = myContract.new({data: contractData, from: testAccount, gas: 1000000})
undefined

# Check out contractInstance has address and hash
> contractInstance
{
  abi: [{
      ...
  }],
  address: "0xef45963f81facfb8cb2a36b350260004a87e05ee",
  transactionHash: "0xace244ce7dcf37b8e0f9e0cc3fbaa1d59d1373882876280b869b457e4868d686",
  ...

```

## Call contract's functions

```
# Save balance status
> var beforeBalance = web3.eth.getBalance(testAccount)
undefined

# Unlock account to call functions
> personal.unlockAccount(testAccount)
Unlock account 0x3419b1faabc867b052483f60de73c78e3dcfd86e
Passphrase: 
true

# In the begining contract instance has a value of 0
> contractInstance.get()
0

# Call the set funtion and set the value of 50
> contractInstance.set(50)
"0x739b3d0c8dc1c2e0e68d67ee89aa12f633a2461504255550ed9d27767101a253"

# New value is 50
> contractInstance.get()
50

# Save balance status
> var afterBalance = web3.eth.getBalance(testAccount)
undefined

# Calculate the cos of calling the function
> var cost = beforeBalance - afterBalance
undefined

> cost
41689000000000

```

# Common issues
## Source file requires different compiler version
Please, look into (official doc)[https://solidity.readthedocs.io/en/v0.5.2/introduction-to-smart-contracts.html] and checkout a compatible script
