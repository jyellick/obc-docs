# A Blockchain Pattern for Industrial Use Cases
**White Paper**  
wip draft 21 - hd/fl/jw - 9/23/15  
Confidential Information  

### SPONSORS (PROJECT BOARD MEMBERS)**IBM**  Gennaro Cuomo, Fellow  John Wolpert, Blockchain Offering Leader  
### PROJECT SNAPSHOT_Electronic networks that transfer the ownership of assets between parties according to business rules are inefficient, expensive and vulnerable to attack.__Blockchains are an emerging technology pattern that can form the fabric of radically improved banking, supply-chain and other transaction networks, giving them new opportunities for innovation and growth while reducing cost and risk._   #### Observations  _No blockchain ‘fabric’ (e.g., Bitcoin, Ethereum, Stellar) is ready for serious business use. The open source community assembling around this whitepaper is defining the specifications, proving assumptions and building reference implementations of the enhancements needed to “make blockchain real for business.”_  #### Structure of the Open Source Community_This work shall be submitted to an appropriate open source foundation (e.g., Linux Foundation, Apache Foundation) project in 2015, and a consortium of companies, key individuals and organizations shall support it._


## Abstract

This paper describes the principles and high-level architecture directions of a blockchain [A] suitable for industrial use cases [B]. With Bitcoin popularizing the domain in 2009, awareness has now reached the point that demand for a solution suitable for industry is surging.
The design presented here provides a platform to leverage blockchain technology for business-to-business and business-to-customer transactions. It is designed for public, private, protected and federated networks, and it allows compliance with regulations and respects the requirements that arise when competing businesses work together on the same network.
The central elements of this proposal are smart contracts, digital assets, a decentralized network and cryptographic security. To these blockchain staples, we add performance, verified identities, private transactions, modular consensus protocols and the utilization of the relationship between validators and account holders.### Why a new fabric
Current blockchain fabrics are not well suited for the needs of the enterprise. Privacy and scalability challenges make their use infeasible for many important industry applications, which they were not designed for. We lay out an industry–focused design, based on and extending the learnings of the pioneers in this field.### Proposed
In this paper, we describe what we believe are the technical features and initial priorities of a new industrial blockchain design. We list what we have found to be key requirements for industry use, based on experience with industry proofs of concept and on review of existing fabrics and innovations. ### Core principles
The focus of this design is to support decentralization (spreading the validation function across many whitelisted legal entities, but not to the point of including anonymous parties), digital assets, and logic (validated, immutable, executable instructions that execute on or off-chain).



### Contents

* I. Design Goals
    * Decentralisation
    * Digital Assets
    * Smart Contracts
* II. Industry Use Cases
    * B2B Contract
    * Asset Repository
    * Medical Records Repository
    * Trade Repository
* III. Industry Requirements  
    * Privacy
    * Finality
    * Identity
    * Transaction
    * Order
    * Scalability and Performance
    * Productivity  
* IV. Industry Trends  
    * Secret Transactions
    * Interacting Blockchains
    * Scalability, Performance, Cost
    * Specialized Payment Transactions
    * Complex Decentralised Application
    * On-Chain Digital Asset Issuance
* Call To the Community
* Notes
* References



## I. Design Goals

The focus of this design for a blockchain for the industry is to support decentralisation, digital assets and smart contracts.


### Decentralisation

Decentralization in this context means sharing state among all nodes in a network and making changes to it in lockstep. Equally, running distributed applications means running an application roughly at the same time on all nodes while coming to a consensus about the result immediately afterwards. Decentralization allows industry players to communicate and enter into contracts without having to agree and compensate a trusted third party to police their trades, and without the need to create custom specifications to interface their IT landscapes. 


### Digital Assets

Digital assets are sets of numbers that represent actual value, tracked on a decentralized ledger. They are in effect numbers that hold bearer value like cash, thanks to decentralization and basic private/public key cryptography. They can be airline miles, fiat currency-backed units of account, securities, deeds, or of any other type. Ideally, e.g. digital securities are issued directly onto the blockchain, allowing trades to clear within milliseconds, rather than within days. It is possible that this will be the standard practice in the future. In the short term, it is likely that digital assets will only representing real world assets. Over time, the cryptographically secured key set itself will become the legal proof of ownership.


### Logic

Blockchain logic, often referred to as “smart contracts,” are self-executing agreements between parties that have all relevant covenants spelled out in code, settle automatically, and can be dependent upon future signatures or trigger events. They cannot be revoked, denied or reversed, because decentralized execution removes them from the control of any one party. They are effectively run on every node at the same time and stopping one on one node just takes the node out of the communication.
Blockchain logic is embodied in verifiable applications that enforce business rules between any parties or devices and can move real money, either by clearing transfers off-chain or settling digital assets on-chain. They can be used to co-ordinate devices in IoT or implement security derivatives with hard coded behavior; they can automate payments, effect conditional ownership change, or transfer permission to use a physical asset.Blockchain logic can also be used to create and run decentralized, autonomous organisations (DAO): a self- governing, asset holding corporation that can trade and extend services, which are relevant because they can fulfill independent, third party business functions, like issuing digital assets.


## II. Industry Use Cases

We have compiled a set of initial blockchain requirements that are considered essential for supporting the following abstract use cases.

### B2B Contract
 
#### Use Case Summary

* Codify business contracts to allow two or more parties to automate contractual agreements in a trusted way.

* Contracts can also be business offers (asks) seeking for bids. This type of contract is usually standardized so that it can be easily searchable by bidders.

 
#### Persona

* Contract participant – Contract counter parties

* Third party participant – A third party stakeholder of a contract who plays the role of guaranteeing the integrity of the contract.

#### Key Components

* Multi-sig contract activation - When a contract is first deployed by one of the counter parties, it will be in pending activation state. To active a contract, signatures from other counterparties and/or third party participants are required.

* Multi-sig contract execution - Some contract will require one of many signatures to execute. (E.g. In trade finance, a payment instruction can only be executed if either the recipient or a authorized third party (e.g. UPS) confirms the shipment of the good)

* Discoverable contract offers - If a contract is a business offer seeking for counter parties (bids), it must also be made searchable. In addition, such contract shall have the intelligence to evaluate bids.

* Payment inside contract- Integration with off-chain payment system is necessary so that contract code can trigger payment transactions that are settled off chain 

* Atomicity of contract execution - Atomicity of the contract is needed to guarantee asset transfers can only occur when payment is received (Delivery vs. Payment). If any step in the execution process fails, the entire transaction must be rolled back

* Contract to chain-code communication - Contracts must be able to communicate with chaincodes deployed on the ledger. E.g. Security lending contracts will call asset contracts to deliver digital assets to counter parties

* Longer Duration contract - Timer is needed to because many B2B contracts may have long execution window. 

* Reuseable contracts - Oftenly used contracts can be standardized so that they can be easily reused. In addition, standardization will improve searchability of contracts.

* Auditable contractual agreement -  Any contract can be made auditable to third parties. E.g. Lawyers may want to audit transactions for legal reasons 

* Contract life-cycle management - B2B contracts are unique and not all of them can be standardized. An efficient contract management system is needed so that contracts will not bloat the system

* Validation access – only nodes with validation right are allowed to validate transactions of a B2B contract, and that could be restricted to stake holders and authorized third party

* View access – B2B contracts may include confidential information, so only accounts with view access right are allowed to view/interrogate them

 
### Manufacturing Supply Chain
 
#### Use case Summary

* Final assemblers, such as car manufactures, can create a supply chain network managed by its peers and suppliers so that final assembler can better manage its suppliers and more responsive to events that would require immediate recalls (triggered by faulty parts supplied by some supplier)


 
#### Persona

* Final Assembler – The business entity that performs the final assembly of a product.

* Part supplier – Suppliers of parts that are used by assemblers. Each supplier can also be an assembler itself, assembled parts it received from sub-suppliers and send the finished product to final assembler

#### Key Components

* Payment upon delivery of goods - Integration with off-chain payment system is necessary to instruct payment instructions upon delivery of a component to the assembler above.

* Third party Audit -  All supplied parts shall be auditable by third parties. E.g. regulators may need to track total number of parts supplied by a supplier for tax accounting purpose

* Obfuscation of shipments - Balances shall be obfuscated so that no one supplier can deduce the exact amount other supplier supplies

* Obfuscation of market size - Total balance must be obfuscated so that part suppliers can’t deduce its market share and use that to seek for better term.

* Validation Access – only nodes with validation right are allowed to validate transactions (shipment of parts)

* View access – only accounts with view access right are allowed to interrogate balances of parts shipped & available.


### Asset Depository 

#### Use Case Summary

* Deploy dematerialized securities on a decentralized ledger network so that all stake holders can access and manage the securities they own or delegated to manage.


 
#### Persona

* Investor – Beneficial owner of securities. Openchain identity manager associates each key pair to a validated identity. Through identity manager, investors can also control and manage the delivery/payment instructions linked to their identity.

* Issuer – Business entity that issued the security which is now dematerialized on the ledger network.

* Custodian – Hired by investors for the safekeeping of their assets, and for providing other services such as asset servicing and optimization. Note that 

* Securities Depository – Depository of dematerialized assets (securities). Known as CSD (Central Securities Depository) in the financial market.   

#### Key Components

* Asset to cash - Integration with off-chain payment system is necessary so that in the case of fixed income securities, issuers can make coupon payments to its investors.

* Reference Rate - Some types of assets such as floating rate notes require access to the reference rates (e.g. LIBOR), such information must be feed into the ledger network.

* Asset Timer - A timer is needed for many types of financial assets that has a life span and have to make periodic payments. E.g. fixed-income securities need to pay periodic payments its investors and need to know what to do do when it reaches maturity.

* Asset Auditor -  Assets must be made auditable to third parties. E.g. regulator may want to audit transactions and movements of assets to measure market risks.

* Obfuscation of account balances - Individual account balances shall be obfuscated so that no one can deduce the exact amount an investor owns.

* Delegation of Control - In case of financial securities, assets (securities) are still registered under each investor’s name (instead of going through a nominee). However, custodian banks will have full control over its managed assets as long as they have access to their clients' keys.

* Validation Access – only nodes with validation right are allowed to validate transactions updating the balances of an asset type (this could be restricted to CSD and/or issuer).

* View access – only accounts with view access right are allowed to interrogate the chaincode that defines an asset type. If an asset represents shares of a publicly traded companies, then the view access could be granted to everyone on the network. 


### Medical Records Repository 

#### Use Case Summary

* One’s medical records are often kept in paper format across many hospitals and local clinics, making them difficult to access and potentially delay one’s medical treatment. With Bylockchain technology, one’s complete health history can be kept on a shared ledger network, enabling healthcare professionals to make more informed decisions about their patients.


### Trade Repository 

#### Use Case Summary

* By design, blockchain is a secure record repository of ordered collection of financial transactions. It records the history of transaction, reducing the need of maintaining a separate trade repository for record keeping.



## III. Industry Requirements

Our investigation with major industry players has resulted in a comprehensive set of requirements that we understand as essential for the success of a blockchain for the industry.


* **Privacy**
	Transaction data for industrial use cases must be private.	Anti-trust regulations are the strongest factor in why privacy has to be stronger for industrial applications, especially in fintech, than in the current public blockchain implementations. Companies are not allowed to share as much information with each other as those systems require.	Obvious reasons of competition are likewise precluding the deployment of current blockchain technology in most business scenarios, which e.g. allows everyone to see every transaction, even though sender and recipient are pseudonymised [c].	Thus currently, observable transaction patterns are too easy to interpret to be acceptable and could e.g. give away details about a  supplier relationship that cannot be revealed.


* **Finality**

	Data stores for industrial use must provide finality, meaning simply that there must be certainty at what point a value is committed and that after this point, it will not change on its own. 	This cannot be guaranteed by the blockchains like Bitcoin and Ethereum, which offer only incremental likeliness of finality over time [1][26], akin eventual consistency [], rather than a firm promise of finality, and can suffer arbitrary loss of committed information as they are designed to allow for, and cope with, forks in their global state.	Enterprise blockchains will instead require immediate and guaranteed finality, based on peer-reviewed and battle-hardened protocols.


* **Identity**

	Many business cases require for identities to be established. Apart from being required by regulations this also allows for reversal of criminal or erroneous transactions, not as built-in mechanism, but by incentive to preserve business reputation and by legal recourse.	Thus, strong identities of both validators and account holders have to be established and managed.
* **Auditability**

	Proof of past records are a natural strength of blockchains, as a side-effect of how consensus is maintained. But auditability for businesses is a requirement going beyond timestamping a state snapshot and includes the identities of actors, which a public blockchain by design hides behind pseudonyms.


* **Transaction Order**

	For industry use, an order of transactions is required that provides market fairness and addresses the selfish miner problems: in Bitcoin or Ethereum, miners can freely decide which transactions to include in what order into a block, or to even not include them at all


* **Scalability and Performance**

	Requirements for scalability and performance vary for industry applications, and a private network does not always have to provide for the expectable load of global all-access use that public blockchains are facing. 

	The more convincingly scalability and performance can be addressed, the broader will the applicability of such a blockchain for the industry be.


* **Productivity**

	Development of decentralised applications and smart contracts is a field where many of the traditional rules of software development hold. But new paths have to be beaten for debugging, testing and maintaining applications in a distributed setting, requiring a holistic approach to the creation of the development tools.


* **Experimentation**

	Especially a new technology like blockchain needs a secure but realistic ways for experimentation, to mount proof-of-concept implementations, and the ability to set up a test environment fast, in private and separated from live business.


### IV. Industry Trends

We draw from the best-of-breed developments, described here, to propose the most powerful and sustainable design possible today.


* **Secret Transactions**

	_Zerocash_ [45][46][47][48][49] is a proposal for an extension of the basic Bitcoin protocol that makes transactions anonymous, as opposed to only pseudonymous. It uses zero-knowledge [50], succinct non-interactive arguments of knowledge (zkSNARKS)[51] to achieve completely secret transactions, shielding sender, receiver and amount, albeit at the severe size and performance panelties coming with the use of zkSNARKS [52].

	Like Sidechains (see below) and the Lightning Network (ditto), Zerocash is designed to be pegged to Bitcoin, creating a parallel mechanism with its advantageous features but using the Bitcoin mainchain as entry and, if desired, exit point. 

	The privacy gained by Zerocash would be limited by a balance model like Ethereum’s [j] but fully viable with any blockchain that is based on a ledger of unspent transactions [83][1][80].


* **Interacting Blockchains**

	The vision of _Sidechains_ [53][54] is a global, decentralised network of multiple blockchains, each with their own features and differing in implementation but all backed by Bitcoin and protected [] by the mining power of the Bitcoin mainnet. The new blockchains’ tokens would be pegged to Bitcoin, so that value can be transferred between the chains, facilitated by _merge mining_ [55]. Sidechains would have to see op codes added to the Bitcoin VM and they would allow the issuance of new digital assets.

	They are seen as a way to re-start the innovation around the original Bitcoin technology [56], which has almost come to a stand-still due to concerns not to endanger the security or the community of the currently running Bitcoin implementation.

	Sidechains’ Elements [54] also address Bitcoin’s privacy problem, their Confidential Transactions [57] conceal the amount being sent in a transaction, though not the sender’s and recipient’s addresses, and at the price of a much larger transaction size.

	We see the concept of interlocking chains as fundamental necessity to make chains interoperable, spread risk and allow for maintenance.


* **Scalability, Performance, Cost**

	The _Lightning Network_ [58][20] is a popular proposal to address the scale, speed and cost problems of the Bitcoin blockchain. It uses a web of micropayment channels that handle small, instant transactions off-chain (as seen from the global Bitcoin mainnet blockchain) and settle an aggregate on-chain at a later point with no counter party risk. This could allow to scale Bitcoin transactions from ~ 3/sec [59] to 100.000/sec [], and in many cases reduce cost of 5 USD Cents per transaction to close to zero, as well as reduce the growth rate of the Bitcoin blockchain.

	While there is no clear path to adoption at this point, this architecture is a precursor of the network of blockchains that Sidechains propose and it is implemented in Sidechains Elements [54].

	We propose an evolution of off-chain transactions that are more integrated with the actual network, by introducing _scope-of-stake_.


* **Specialized Payment Transactions**

	_Ripple_ [60] is a real-time settlement system and currency exchange network built upon a distributed, Open Source Internet protocol [61] and a consensus ledger, with a strong focus on financial transactions. Its native crypto-currency, XRP, is the second-largest by market capitalisation [62] after Bitcoin. Efforts to allow for more versatile smart contracts, creating the off-chain Codius [63][64] have been abandoned [65].

	Any new proposal in the blockchain space has to be able to implement Ripple’s core features on a smart contract–basis, like e.g. Ethereum is.


* **Complex Decentralised Application**

	Even the programmability and versatility of the Ethereum VM, which far surpasses that of Bitcoin, is too limited, not performant enough and can be difficult to use when designing more complex systems.

	_Augur_ [66], a prediction market implemented on Ethereum, demonstrated that at least for cryptographic functions, it is desirable to be able to use existing libraries and have them execute with as much performance as possible.

	Ethereum was conceived mostly with specialised, but at the heart simple fintech transactions in mind, allowing for innumerable customisations for financial transactions. Ethereum’s specification [9] and implementation realised a more generalised product. But it is becoming apparent that the scope of ideas people bring to the space reaches far beyond this initial focus, and demands a more general and powerful VM to execute smart contracts.


* **On-Chain Digital Asset Issuance**

	Two recent products unveiled at Wallstreet, t0 [4] and Smart Securities [5], as well as the collaboration of Nasdaq with Chain.com [68], underline a shift in the industry efforts to realizing the advantages in publishing digital assets directly onto the blockchain, using the Bitcoin blockchain for settling trades within minutes. While the results demonstrated are magnitudes faster, purportedly at 10 minutes for settling, than the current settling time of three days, the presented applications are focused narrowly on exchange-related use-cases and they suffer from the lacking finality guarantee of Bitcoin’s forking consensus model.

	We are proposing a design that allows for settling within the second, and which, unlike a Bitcoin-based solution, is immediately final.



### Call To the Community

We believe that blockchain technology is nothing short of revolutionary and will democratise and transform commerce, in the way highways or the advent of the internet did.

Join us to create the most powerful interplay between smart contracts and digital assets, set up in the new, distributed way that has put many established wisdoms of database science on its head.

For digital assets to be secure, they need to be issued into a proven and hardened blockchain infrastructure, made for purpose rather than adapted as good as possible. Safety and finality guarantee will drive the adaption of the practice to issue e.g. securities or deeds directly on the blockchain.

We are starting with a clear premise, rejecting sacrifices that we don’t need to make and embracing the reality of the enterprise domain, like high-powered hardware, industrial network protection, identification by name and address of all participants, and the essential pattern of the customer relationship between transactors and validators.

We make strong choices, like sticking with the Bitcoin transaction model and replacing a ground-breaking innovation of blockchains – the fork-choice rule – with a better suited protocol, to achieve private transactions with guaranteed consistency.

We will need to draw from all these assets to succeed in building a new blockchain that allows for the performance and scale that big markets and complex business logic require.

This will make this platform the technology of choice for business consortiums and industry groups to stand up new exchanges, trade routes and channels that serve provably correct, up to date information.

Your feedback and participation in this Open Source effort is highly appreciated.




## Notes

[a] A *blockchain* is generally understood as the technology driving the crypto-currency Bitcoin. Definitions beyond this common denominator vary. The name is derived from the fact that groups of transactions, the blocks, include a hash of a previous block in their own hash, which creates a string of blocks where each always includes a hash of all previous blocks, chaining them together. The benefit is that it is immediately detectable when a block in the chain is altered.

[c] Bitcoin and Ethereum accounts, or addresses, are *pseudonymised* but not anonymous. The ownership is indicated by a large number, the public key of a private-public key pair, which is openly visible to anyone at any time, for every transaction, present and past.

[d] *off-chain payment* means, handling the conditions of a smart contract using a blockchain, but eventually making the payment through an interface to established channels, e.g. a a credit card transaction acquirer.  

[e] *Elasticity* describes the capability of a network to allow for new nodes to be added in on the fly, without stopping or restarting it.  

[f] *High Availability* means that a database cluster – which a blockchain is – can be accessed even after losing many or almost all nodes after a catastrophic event.  

[g] Bitcoin was *crowd funded* by means of mining rewards, paid in Bitcoin, which provided incentive to run nodes and grow the network. Ethereum held a pre-sale of Ether, which is its native crypto-currency and the tender to buy computation and storage time for anything one wants to transact on the global Ethereum network. This use of Ether is called _gas_.

[i] *Double-spending* is a basic challenge for digital currencies: if value is represented by digital numbers, they can obviously be copied and through that be spent more than once. Before Bitcoin, the only known solution to this was a centralised account system keeping track of all transfers.

[j] Bitcoin chains hashes of transactions only, and balances of accounts can be calculated on a need-know basis by clients, from the entirety of the transactions. Ethereum hashes balances of accounts, too, to be able to prune transaction as a path to better scale. But hashing balances means that the _results_ of transactions are verified by all validators and thus, need to be shared and agreed upon by all validators, which defeats zero-knowledge.

[k] *Performance differences* between centralised and decentralised data storage systems are roughly at 6 magnitudes. Bitcoin can handle around 3 transactions a second [59] while a specialised high throughput database like VoltDB can transact millions.  

[l] *Proof-of-work* is the ‘math-riddle’ scheme that Bitcoin and Ehtereum 1.0 use to secure the network and allow for all nodes to agree on a block proposer. Concretely, a valid hash of a set of transaction data has to be found, by adding a random nonce, that co-incidently has a certain required number of leading zero bits. The transactions, hash and nonce are then presented to other nodes, as proof that the proposer has done the work the find the nonce, which can only be by trying nonces blindly until one yields the desired zero-leading hash.  

[m] *Verification* is done by calculating the given transactions in a block against the initial state and checking that the resulting state is the same as the proposer of the block claims.  

[n] A *permissioned* ledger is one that is not public but accessible only through, usually centralised, access control, which is fundamentally different from what e.g. Bitcoin and Ethereum were made for.  

[o] A *distributed* system is one in which fully concurrent nodes interact with each other in order to achieve a common goal and coordinate their actions by passing messages to each other.  

[p] *Ether* is the native crypto-currency of Ethereum, the chain’s *native token*.

[q] Miners secure the network as per Bitcoin’s design by spending massive calculation power on solving hashes ‘backwards’ [l], with hashes chained in a way that makes it very hard to retro-actively change any state on the blockchain, as all later state would also have to be changed, and all their hashes re-calculated, in the same high-effort, backward way.  

[r] Bitcoin and Ethereum do not actually *elect leaders* but implement a competition among nodes [l] who can propose a block and the necessary _proof-of-work_ first. This is unpredictable and thus makes it impossible to focus a DDOS attack on any node that predictably will be the respective next leader, to bring the network down.  

[s] Auditability is a side effect of the way that Bitcoin and Ethereum store state in the open and with a timestamp that the entire miner network signs off on. Since it is easy to verify that a pseudonym belongs to someone who claims so, all transactions on the entire blockchain can be verified by any third party.  



## References

[1] Bitcoin whitepaper, Satoshi Nakamoto 2008 – https://bitcoin.org/bitcoin.pdf  

[2] Bitcoin community site – https://bitcoin.org/en/  

[3] Bitcoin at Wikipedia – https://en.wikipedia.org/wiki/Bitcoin  

[4] Fintech startup t0: http://www.t0.com  

[5] Fintech startup Symbiont, Smart Securities – http://symbiont.io  

[6] Fintech startup Chain – http://chain.com  

[7] ‘Smart Contracts Land on Wall Street’, Bitbeat 2015 – http://blogs.wsj.com/moneybeat/2015/08/05/bitbeat-smart-contracts-land-on-wall-street/  

[8] Ethereum whitepaper, Vitalik Buterin 2013 – https://github.com/ethereum/wiki/wiki/White-Paper  

[9] Ethereum specification yellow paper, Gavin Wood 2014 – http://gavwood.com/Paper.pdf  

[10] Ethereum official site – https://www.ethereum.org  

[11] Ethereum at Wikipedia – https://en.wikipedia.org/wiki/Ethereum  

[12] Ripple whitepaper – https://ripple.com/files/ripple\_consensus\_whitepaper.pdf  

[13] Ripple official site – https://ripple.com  

[14] Ripple at Wikipedia – https://en.wikipedia.org/wiki/Ripple\_(payment\_protocol)  

[15] ‘Protect your privacy’ – https://bitcoin.org/en/protect-your-privacy  

[16] ‘Bitcoin offers privacy—as long as you don't cash out or spend it’, PCWorld 2013 – http://www.pcworld.com/article/2047608/bitcoin-offers-privacy-as-long-as-you-dont-cash-out-or-spend-it.html  

[17] Ethereum does not address privacy – https://forum.ethereum.org/discussion/1925/financial-privacy-with-ethereum  

[18] Anonymity not a design goal of Ripple – https://wiki.ripple.com/FAQ#How\_does\_Ripple\_handle\_privacy.3F   

[19] Bitcoin scalability – https://en.bitcoin.it/wiki/Scalability   

[20] Lightning Network – http://lightning.network/lightning-network.pdf  

[21] ‘Ethereum Scalability and Decentralization Updates’, Vitalik Buterin 2014 – https://blog.ethereum.org/2014/02/18/ethereum-scalability-and-decentralization-updates/  

[22] ‘Scalability, Part 2: Hypercubes’, Vitalik Buterin 2014 – https://blog.ethereum.org/2014/10/21/scalability-part-2-hypercubes/  

[24] ‘Device Democracy’, IBM Institute for Business Value 2014 – http://www-935.ibm.com/services/us/gbs/thoughtleadership/internetofthings/  

[25] ‘Empowering the Edge’, IBM Institute for Business Value 2015 – http://public.dhe.ibm.com/common/ssi/ecm/gb/en/gbe03662usen/GBE03662USEN.PDF  

[26] Bitcoin transaction confirmation – https://en.wikipedia.org/wiki/Bitcoin\_network#Transaction\_confirmation  

[27] Bitcoin block height and forking, Bitcoin developer guide – https://bitcoin.org/en/developer-guide#block-height-and-forking  

[28] Ethereum GHOST protocol – https://www.cryptocompare.com/coins/guides/what-is-the-ghost-protocol-for-ethereum/  

[29] Blockstream official site – https://blockstream.com/about/  

[30] ‘b-money’, W. Dai 1998 – http://www.weidai.com/bmoney.txt    

[31] ‘Design of a secure timestamping service with minimal trust requirements’, H. Massias, X.S. Avila, and J.-J. Quisquater 1999 – 20th Symposium on Information Theory in the Benelux  

[32] ’How to time-stamp a digital document’, S. Haber, W.S. Stornetta, 1991 – Journal of Cryptology, vol 3, no2, pages 99-111  

[33] ‘Improving the efficiency and reliability of digital time-stamping’, D. Bayer, S. Haber, W.S. Stornetta, 1993 – Sequences II: Methods in Communication, Security and Computer Science, pages 329-334  

[34] ’Secure names for bit-strings’, S. Haber, W.S. Stornetta, 1997 – Proceedings of the 4th ACM Conference on Computer and Communications Security, pages 28-35  

[35] ‘Hashcash - a denial of service counter-measure’, A. Back 2002 – http://www.hashcash.org/papers/hashcash.pdf  

[36] ‘Protocols for public key cryptosystems’, R.C. Merkle 1980 – Proc. 1980 Symposium on Security and Privacy, IEEE Computer Society, pages 122-133  

[37] ‘An introduction to probability theory and its applications’ – W. Feller 1957  

[38] Bitcoin Script language – https://en.bitcoin.it/wiki/Script  

[39] Bitcoin standard transaction – https://en.bitcoin.it/wiki/Transaction#Pay-to-PubkeyHash  

[40] ‘Smart Contracts’, Nick Szabo 1997 – http://szabo.best.vwh.net/smart\_contracts\_idea.html  

[41] Bitcoin multi signature escrow example – https://en.bitcoin.it/wiki/Contract#Example\_2:\_Escrow\_and\_dispute\_mediation  

[42] Ethereum VM – https://ethercasts.github.io/training/ethereum\_virtual\_machine.html#/  

[43] Ethereum high level language Solidity – https://github.com/ethereum/wiki/wiki/The-Solidity-Programming-Language  

[44] Ethereum VM 256 bit stack - https://ethercasts.github.io/training/ethereum\_virtual\_machine.html#/13  

[45] Zerocash extended whitepaper – http://zerocash-project.org/media/pdf/zerocash-extended-20140518.pdf  

[46] Zerocash official site – http://zerocash-project.org  

[47] ‘How Zerocash works’, Zerocash Project 2015 – http://zerocash-project.org/how\_zerocash\_works  

[48] Zerocash at Wikipedia – https://en.wikipedia.org/wiki/Zerocoin#Zerocash    

[49] Zerocoin (sic) rationale at Wikipedia – https://en.wikipedia.org/wiki/Zerocoin#Rationale    

[50] Zero-knowledge proofs, Wikipedia – https://en.wikipedia.org/wiki/Zero-knowledge\_proof  

[51] ‘SNARKs for C :Verifying Program Executions Succinctly and in Zero Knowledge’ – https://eprint.iacr.org/2013/507.pdf    

[52] Cost of zkSNARKs at Wikipedia – https://en.wikipedia.org/wiki/Zerocoin#Criticism  

[53] Pegged Sidechains whitepaper – https://www.blockstream.com/sidechains.pdf  

[54] Blockstream Sidechain Elements – https://github.com/ElementsProject/elementsproject.github.io  

[55] Merge Mining, Bitcoin docs – https://en.bitcoin.it/wiki/Merged\_mining\_specification  

[56] Founders’ rationale, Blockstream official site – https://www.blockstream.com/2014/10/23/why-we-are-co-founders-of-blockstream/  

[57] ‘Confidential Transactions’, Blockstream Sidechains Elements – https://github.com/ElementsProject/elementsproject.github.io/blob/master/confidential\_values.md  

[58] Lightning Network whitepaper – http://lightning.network/lightning-network-paper.pdf  

[59] Bitcoin performance – http://hashingit.com/analysis/33­7­transactions­per­second    

[60] ‘How Ripple works’, Ripple official site – https://ripple.com/knowledge\_center/how-ripple-works/  

[61] Ripple consensus protocol whitepaper – https://ripple.com/files/ripple\_consensus\_whitepaper.pdf  

[62] Crypto-currency Market Capitalisations, coinmarketcap.com – http://coinmarketcap.com  

[63] Codius whitepaper – https://github.com/codius/codius/wiki/Smart-Oracles:-A-Simple,-Powerful-Approach-to-Smart-Contracts  

[64] Codius official site – https://codius.org  

[65] ‘The Quiet Death of Ripple’s Codiu§ Project’, Florian Glatz – https://medium.com/@heckerhut/the-quiet-death-of-ripple-s-codiu-project-782c11a17c02  

[66] Augur official site – http://www.augur.net  

[67] ‘Smart Contracts Land on Wall Street’, Bitbeat – http://blogs.wsj.com/moneybeat/2015/08/05/bitbeat-smart-contracts-land-on-wall-street/  

[68] ‘Nasdaq and Chain to Partner on Blockchain Technology Initiative’, Nasdaq – http://www.nasdaq.com/press-release/nasdaq-and-chain-to-partner-on-blockchain-technology-
initiative-20150624-00446  

[69] Transaction malleability, Bitcoin docs – https://en.bitcoin.it/wiki/Transaction\_Malleability  

[70] Downsides of (ASICs) mining, Bitcoin docs – http://en.bitcoinwiki.org/ASIC\_mining#Downsides\_of\_mining\_business  

[71] ‘Contracts’, Bitcoin docs – https://en.bitcoin.it/wiki/Contract  

[72] Solidity example, Order statistic tree, Conrad Bars - https://github.com/drcode/ethereum-order-statistic-tree/blob/master/OrderStatisticTree.sol  

[73] Sybil attacks, Bitcoin docs – https://en.bitcoin.it/wiki/Weaknesses#Sybil\_attack  

[74] ‘Block Chain’, Bitcoin docs – https://en.bitcoin.it/wiki/Block\_chain  

[75] Bitcoin forks are hard to detect – https://bitcointalk.org/?topic=6109.0  

[76] March 2013 Chain Fork Post-Mortem, Bitcoin BIPs – https://github.com/bitcoin/bips/blob/master/bip-0050.mediawiki  

[77] ‘Bitcoin Network Shaken by Blockchain Fork’, Bitcoin Magazine 2013 – https://bitcoinmagazine.com/3668/bitcoin-network-shaken-by-blockchain-fork/  

[78] CAP at Wikipedia – https://en.wikipedia.org/wiki/CAP\_theorem  

[79] Ethereum Design Rationale, Ethereum docs – https://github.com/ethereum/wiki/wiki/Design­-Rationale    

[80] Accounts vs UTXOs, Ethereum docs – https://github.com/ethereum/wiki/wiki/Design-Rationale#accounts-and-not-utxos  

[81] Byzantine fault tolerance, Wikipedia – https://en.wikipedia.org/wiki/Byzantine\_fault\_tolerance  

[82] ‘H-Store: A High-Performance, Distributed Main Memory Transaction Processing System’, Brown University 2008 – http://cs-www.cs.yale.edu/homes/dna/papers/hstore-demo.pdf  

[83] Bitcoin transactions, Bitcoin docs – https://en.bitcoin.it/wiki/Transaction

[84] Visa, official about page – http://usa.visa.com/about-visa/our-business/visa-transaction.jsp



## Authors

Henning Diedrich, hdiedrich@de.ibm.com
Frank Lu, fylu@us.ibm.com
John Wolpert, jwolpert@us.ibm.com



## Changes

**21** fl/jw: use cases; sponsors; observations; abstract; shortening; truncated section II; cut sections III and IV, Acknowledgements

**19** hd/sb: clarifications; Experimentation; Fund Lock-In; Maintenance; references

**18** hd: section II structure re-ordering; shortening; wording; formatting

**17** hd: references; design goal explanations; wording; typos

**16** jc/hd: index; clarifications; shortening

**15** jc/hd: clarifications; bullet formatting; notes

**14** jc/hd: section order back to draft 6  

**13** hd: reference and notes placeholders; references

**12** hd: slight shorting, wording, subchapter order second half

**11** hd: on-chain code; sidechains; shorting, wording, subchapter order first half

**10** jc/hd: examples; explanations; summary as call to action

**9** hd: section structure; flow

**8** hd: section heads; wording

**7** jw/hd: section order; rephrasing; spelling

**6** fl/hd: deployment; ‘permissioned’; drop Augur; rephrasing 

**5** gs/hd: errors; clarifications; grammar; spelling; references

**4** jw/hd: add clarifications; spelling

**3** add 'Confidential Information'

**2** add Async Calls; rewrite smart contracts, sidechains, zerocash; add to Tenets; headline rewrites in part I; paragraph spacing; list formatting; headline capitalisation; minor rewording


