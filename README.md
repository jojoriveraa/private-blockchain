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

# Setup
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

# Geth JavaScript console
- web3.eth.accounts: Return a list with available accounts
- web3.eth.getBalance("[Account-hash]"): Return the balance of given account

