---
description: An anonymous login system using Bitcoin
---

# 🆔 NullPass

## Login Flow

Users can use a Bitcoin wallet address or a Lightning Node to login.&#x20;

### 📃 Wallet

![](../.gitbook/assets/LWBW.png)

{% hint style="info" %}
Login is achieved by verifying a signature against a wallet address.
{% endhint %}

Copy the generated message into your wallet. We will use BlueWallet as an example.

### ⚡ Lightning Node

![](../.gitbook/assets/LWLN.png)

{% hint style="info" %}
Login is achieved by extracting the public key from a signature and checking for a node heartbeat.
{% endhint %}
