# Metacrafters 

This project contains code for the Final Assessment of for the course Building on Avalanche - ETH + AVAX.

## Description

The goal of the project is to create my own ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract has the following functionality:

- Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
- Transferring tokens: Players should be able to transfer their tokens to others.
- Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
- Checking token balance: Players should be able to check their token balance at any time.
- Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

## Getting Started

### Executing program

In order to run this program, it is recommended to copy the whole code and run it in Remix or HardHat. The code can be copied below:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {

    mapping(uint256 => uint256) public prices;

    constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {
        prices[1] = 100;
        prices[2] = 50;
        prices[3] = 25;
    }

    function mint(address _to, uint256 _value) public onlyOwner {
        _mint(_to, _value);
    }

    function transferTokens(address _to, uint256 _value) external {
        require(balanceOf(msg.sender) >= _value, "Error: You do not have enough Degen Tokens");
        approve(msg.sender, _value);
        transferFrom(msg.sender, _to, _value);
    }

    function redeemToken(uint8 _option) external {
        require(0 <= _option && _option <= 3, "Please select a valid item.");
        require(balanceOf(msg.sender) >= prices[_option], "Insufficient funds to redeem the item.");
        transfer(owner(), prices[_option]);
    }

    function burnTokens(uint256 _value) external {
        require(balanceOf(msg.sender) >= _value, "Error: You do not have enough Degen Tokens");
        approve(msg.sender, _value);
        _burn(msg.sender, _value);
    }

    function showShopItems() external pure returns (string memory) {
        string memory text =
            "The items on sale are the following: [1] Degen NFT (100) [2] Degen Item (50) [3] Official Degen Merch (25)";
        return text;
    }

    function decimals() override public pure returns (uint8) {
        return 0;
    }

    function getBalance() external view returns (uint256) {
        return this.balanceOf(msg.sender);
    }


}
```

After copying it, compiling it will make the contract deployable and ready for interaction.

## Submitted by:

John Felix M. Agda
202011499@fit.edu.ph
BSCSSE