---
description: Renounce to address(0) no more.
---

# 💠 Contract Vault

## ⚠️ Disclaimer <a href="#9b9c" id="9b9c"></a>

{% hint style="info" %}
The Vault is completely autonomous and decentralized. Nullbit <mark style="color:red;">**cannot**</mark>** ** claim any smart contract ownership locked in the Vault.

The Vault <mark style="color:red;">**cannot**</mark>** ** interact or make any sort of calls to the renounced smart contract.

The Vault's audit report can be found **here**.
{% endhint %}

## The address(0) Problem

In the blockchain world, renouncing ownership of a contract is where a contract owner simply assigned ownership of the contract to the zero address, thus removing ownership from themselves. This essentially sets the ownership of the contract to nobody, as nobody owns the private keys to the zero address.

> Renouncing ownership protects the community from the owners of the contract from acting maliciously.

Although the above statement can sometimes be true, it only constitutes one side of the story. If a team renounces ownership this means no further development or changes can be carried out to the contract. And by renouncing to the zero address, it means there is no way for the team to get back ownership. This can also signify that a team does not have long-term plans for their project.

## The Contract Vault

We wanted to develop a completely decentralized way for teams to renounce ownership of their contracts so that their respective communities feel safe, but at the same time help them recover ownership back to make adjustments and changes as they see fit.

### How does it work

Our Vault is a smart contract that runs on all our supported blockchains. When a team wants to renounce ownership, they transfer ownership rights to the Vault smart contract along with a restorable date.

If a team requires the ownership to be transferred back to them for any reason, they simply need to wait for the time to elapse and then pay the Vault the required **fee**. The ownership will then automatically be transferred back to the previous owner.

{% hint style="danger" %}
The **ONLY** address that can reclaim ownership of a contract stored in the Vault is the one that initiated the transfer in the first place. In other words, **ONLY** the previous owner can get ownership of a stored contract in the Vault.
{% endhint %}

### How to lock

WIP

### How to restore

When the restorable date has passed, the team can reclaim ownership of their smart contract by providing its address. The required payable fee is also displayed. \
A metamask prompt will automatically populate with the required fee to be paid for the transfer. \
Once the transfer is completed the vault will automatically transfer ownership back to the previous owner.

### Check

The check tab is there for anyone to interact with the vault and query any smart contract's status. Simply provide the locked contract's address and the vault will display its restorable date.

## Requirements

When teams want to initiate an ownership transfer to the Vault, they need to make sure that their smart contract abides with the requirement.

We have modified the OpenZappelin's [**Ownable.sol**](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol). Teams need to make sure that their smart contracts inherit from our **CVOwnable.sol** instead. Simply replace the `contract Ownable is Context` block from your smart contract with the code below depending on your deployment environment:

```solidity
// SPDX-License-Identifier: MIT License
// nullbit.io

pragma solidity 0.8.15;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * It also provides the functionality to store the contract in nullbit's Contract Vault
 * using {storeInVault}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
 
 abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

contract CVOwnable is Context {
    address private _owner;
    uint256 private _unlockTime;
    address private _previousOwner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "CVOwnable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }
    
    /**
     * @dev Returns the address of the previous owner.
     */
    function previousOwner() public view returns(address) {
        return _previousOwner;
    }
    
    /**
     * @dev Returns the unlock time of the contracted stored in the Vault.
     */
    function getUnlockTime() public view returns(uint256){
        return _unlockTime;
    }


    /**
     * @dev Transfers ownership of the contract to nullbit's Contract Vault (`Vault`) and
     * sets the time (`unlockTime`) at which the now stored contract can be transferred back to
     * the previous owner.

     * NOTE Can only be called by the current owner.
     */
    function storeInVault(address Vault, uint256 unlockTime) public virtual onlyOwner {
        require(Vault != address(0), "CVOwnable: zero address");
        _previousOwner = _owner;
        _unlockTime = unlockTime;
        emit OwnershipTransferred(_previousOwner, Vault);
        _owner = Vault;
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).

     * NOTE Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "CVOwnable: zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        _previousOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(_previousOwner, newOwner);
    }

}
```

{% hint style="warning" %}
If you get a `SyntaxError: No visibility specified` error when compiling, add`public`to the constructor at line 43 of the CVOwnable.sol like so: `constructor() public {`

If your contract already contains the `abstract contract Context {}` then make sure you remove it from the CVOwnable.sol as to not cause duplication errors.


{% endhint %}

{% hint style="info" %}
If you require assistance implementing the Contract Vault to your smart contract, please contact us via our social accounts found [**here**](../connect.md#internet-presence). A community member or an admin will be more than happy to assist you.
{% endhint %}

## Benefits

Renouncing contract ownership to the Vault prevents fraudulent activities from happening. It also shows that the team behind the project is highly interested in the projects' long-term viability, boosting investors' confidence.

🛡️ Provides security to the communities.

♻️ Allows teams to restore ownership if they need to further develop or change anything in their contract.

👮🏻 Prohibits ownership restoration abuse as the teams need to pay a significant fee for the ownership to be restored.

## Codebase

| GitHub Repository     | WIP                                    |
| --------------------- | -------------------------------------- |
| Version               | 0.1.0                                  |
| Supported Blockchains | <p>Ethereum<br>Binance Smart Chain</p> |
| Audit                 | WIP                                    |
