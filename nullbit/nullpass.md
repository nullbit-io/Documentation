---
description: An anonymous login system using Bitcoin.
---

# 🆔 NullPass

## Login Flow

Users can use a Bitcoin wallet address or a Lightning Node to login.&#x20;

### Wallet

![](../.gitbook/assets/LWBW.png)

Login is achieved by verifying a signature against a wallet address. If the signature is valid, the system uses the users wallet address as the login credential.

### Lightning Node

![](../.gitbook/assets/LWLN.png)

Login is achieved by extracting the public key from a signature. If the node that signed the message is visible, the system uses the nodes public key as the login credential.
