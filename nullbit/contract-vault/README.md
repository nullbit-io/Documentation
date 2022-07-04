---
description: Renounce to address(0) no more
---

# 💠 Contract Vault

## ⚠️ Disclaimer <a href="#9b9c" id="9b9c"></a>

{% hint style="info" %}
The Vault is completely autonomous and decentralized. Nullbit <mark style="color:red;">**cannot**</mark>** ** claim any smart contract ownership locked in the Vault.

The Vault <mark style="color:red;">**cannot**</mark>** ** interact or make any sort of calls to the renounced smart contract.
{% endhint %}

## Get Started

| 📦 GitHub Repository | WIP                                    |
| -------------------- | -------------------------------------- |
| 🛂 Code Audit        | WIP                                    |
| 🔗 Vault Locations   | <p>Ethereum<br>Binance Smart Chain</p> |

## The address(0) Problem

In the blockchain world, renouncing ownership of a contract is where a contract owner simply assigned ownership of the contract to the zero address, thus removing ownership from themselves. This essentially sets the ownership of the contract to nobody, as nobody can own the private keys to the zero address.

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

### How to check

The check tab is there for anyone to interact with the vault and query any smart contract's status. Simply provide the locked contract's address and the vault will display its restorable date.

## Benefits

Renouncing contract ownership to the Vault prevents fraudulent activities from happening. It also shows that the team behind the project is highly interested in the projects' long-term viability, boosting investors' confidence.

🛡️ Provides security to the communities.

♻️ Allows teams to restore ownership if they need to further develop or change anything in their contract.

👮🏻 Prohibits ownership restoration abuse as the teams need to pay a significant fee for the ownership to be restored.

## Fees

WIP

## Limitations

WIP
