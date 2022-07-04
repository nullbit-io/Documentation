# Implement The Vault

When teams want to initiate an ownership transfer to the Vault, they need to make sure that their smart contract abides with the requirement.

We have modified the OpenZappelin's [**Ownable.sol**](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol). Teams need to make sure that their smart contracts inherit from our **CVOwnable.sol** instead. Simply replace the `contract Ownable is Context` block from your smart contract with the code below depending on your deployment environment:

```solidity
// Some cod// SPDX-License-Identifier: MIT License
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

}e
```

{% hint style="warning" %}
If you get a `SyntaxError: No visibility specified` error when compiling, add`public`to the constructor at line 43 of the CVOwnable.sol like so: `constructor() public {`

If your contract already contains the `abstract contract Context {}` then make sure you remove it from the CVOwnable.sol as to not cause duplication errors.
{% endhint %}

{% hint style="info" %}
If you require assistance implementing the Contract Vault to your smart contract, please contact us via our social accounts found [**here**](../../contact-us.md#internet-presence). A community member or an admin will be more than happy to assist you.
{% endhint %}
