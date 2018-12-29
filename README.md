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
