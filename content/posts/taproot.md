---
title: "ELI5: Taproot activation"
date: 2021-12-08T13:33:37Z
---

The aim of this post is to provide a brief summary, of the implications that come with
the adoption of Bitcoin's recent Taproot softfork. Its aim is to optimize Bitcoin's  
scalability, privacy, and smart contract functionality.

I got to learn about the nitty gritty details that are involved with the Bitcoin
protocol whilst working for Bitonic, a BTCEUR exchange with proper moral values. Do note,  
I can't stress enough the importance of the DYOR (Do Your Own Research) idiom.

The softfork introduces a new address type (P2TR), that aims to:

- Reduce the size of complex transactions.
  - This improves Bitcoin's scalability.
- Complex transactions will be (in most cases) indistinguishable from simple ones.
  - This improves Bitcoin's transactional privacy.
- Support for more complex transactions.
  - Better smart contract functionality.

Spending conditions of these 'complex transactions' could be:

- Transactions that originate from multisignature addresses (aka wallets that are owned
  by multiple parties).
  - "Only transact if child AND /both/ parents agree."
- Transactions with, one or more timelocked spending condition(s).
  - "Only transact if child reaches the age of 21."
- Transactions that combine the features stated above, accompanied by (optional)
  fallback condition(s).
  - "Only transact if child reaches the age of 18 AND /both/ parents agree, OR if child
    reaches the age of 21."
