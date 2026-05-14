# Tech features
## Key Images
One important property of monero is "key images" - these are deterministic messages that are non-back-computable. Basically when a UTXO is spent, a "key image" is generated. There is only one key image for each UTXO, but it cannot be linked to the UTXO (except by the UTXO owner). The result is that the network can scan for a matching key image to prevent double-spends.

There are likely ways to improve the current state of the [[Comfy]] Dapp, which reveals sender and recipient on every transfer.

## Dandelion
- According to [moneroleaks.xyz](https://moneroleaks.xyz/), dandelion transactions are not encrypted and are passed from node-to-node until they are broadcast. 
	- [ ] #OpenQuestion is it possible to disrupt Dandelion by simply broadcasting all transactions which come to your node? might reduce privacy somewhat
		- *However, the Dandelion++ protocol has weaknesses, including that is based on probability: a node has a certain percentage chance it will act as a stem node and forward the transaction to just one peer. The rest of the time, it acts as a "fluff node" and broadcasts the transaction to all of its peers. An attacker can exploit this: when he receives a transaction from a peer, he can query other nodes for the transaction id to "find out" if the previous node was in the stem phase or the fluff phase. If, after a delay, no one has the transaction, the attacker knows the transaction is likely still in its "stem phase." That information allows for further analysis: if we are still in the stem phase, then the prior node was definitely not a fluff node. It was either a stem node or the sender, which is data the attacker should not have.*
			- From moneroleaks.xyz
		- Seems like a good reason to run a node! 
## Post-quantum resistance
- This is still a planned feature (as of may 13 2026) 
- see [this discussion on the monero research lab github](https://github.com/monero-project/research-lab/issues/151#issuecomment-4412416686)  
- CARROT includes some improvements to quantum resistance, such that quantum enabled adversaries will not be able to construct transaction graphs as easily
	- based on [this comment](https://github.com/monero-project/research-lab/issues/151#issue-3548827396), it appears that the added difficulty here stems from how public keys are concealed.
```
However, if a QEA learns just one wallet address, they can deanonymize a large portion of the wallet's transaction history, including:

1. All incoming external enotes with their amounts.
2. In some cases, the spends of such enotes (this requires at least 2 enotes received to the same wallet address to be spent).
```

## Jamtis
* Looks like the point of Jamtis is to provide post-quantum protection (see  [this comment](https://github.com/monero-project/research-lab/issues/151#issue-3548827396) on github)
```
The purpose of Jamtis is to provide forward secrecy against a QEA even if a wallet address is publicly known or under a stronger assumption that the address-generator wallet tier is leaked.
``` 

# Community
## Chats 
- libera chat is where monero research lab meets 
	- https://web.libera.chat/ - browser client for IRC
	- channels: 
		- `#monero-research-lab`
		- `#monero-research-lounge` for more casual chats
		- `#monero-dev`
- #TODO there's also a matrix chat
	- https://matrix.to/#/#monero-research-lab:monero.social?via=matrix.org&via=monero.social

## Announcements 
- You can sign up for the [announcement listserv](https://lists.getmonero.org/) 
	- #TODO does this link still work? Its from the `monero-project/monero` Github repo README. But it wasn't loading for me. Got a 504 timeout

## Githubs and such
- [monero protocol github](https://github.com/monero-project/monero) 
- [monero "meta" github](https://github.com/monero-project/meta/issues)
	- This has info about the monero research lab meetings and stuff
- [monero research lab github](https://github.com/monero-project/research-lab/)

## Contributing
- There's a push for translating the monero CLI wallet into different languages
	- https://translate.getmonero.org/projects/monero/cli-wallet/
	- #TODO does this link work? Got a 502 error. Is from the `monero-project/monero` README
- As of may 13 2026, Coverage is only at 33% on the `monero-project/monero` repo. This could likely be improved