# Industry Blockchain
**White Paper**  
wip draft 19 - hd/sb - 9/21/15  
Confidential Information  




## Abstract

There is tremendous opportunity in creating a blockchain [a] for the enterprise, with features that satisfy the requirements of the industry, building on the existing experience in this emergent space. With Bitcoin [1][2][3] creating the domain in 2009, awareness has now reached the point that demand for a solution suitable for the industry is surging. As many mainstream fintech-geared startups announcing Bitcoin-based or improvised solutions [4][5][6][7], the time for an integrated new approach is now.

The design presented in this paper provides a platform to leverage blockchain technology for inter-business, intra-business, and business-to-customer transactions. It is made for private, protected, federated networks, allows compliance with regulations and respects the requirements that arise when competing businesses are to work together.


The central elements of this proposal are smart contracts, digital assets, a decentralised network and cryptographic security. To these blockchain staples, we are adding performance, verified identities, privacy of transactions, a consistent, fault tolerant consensus protocol and the utilisation of the 1:n relationship between validators and account holders, e.g. banks in a private network that each run a validator, while each account in the blockchain is associated with one bank account, associated with one bank.

We believe the current blockchain technology – as present in Bitcoin, Ethereum [8][9][10][11] or Ripple [12][13][14] – is not well suited for the needs of the enterprise. The much discussed, existing privacy [15][16][17][18] and scalability challenges [19][20][21][22] make their use unfeasible for many plausible industry applications, which they were not designed for. In this paper we lay out an industry–focussed design, based on the learnings of the blockchain pioneers and thought to complement them in the vast space of opportunity for this new technology.

In this paper, we deduce the technical features of a new blockchain design. We list what we have found to be base requirements for industry use and examine the current situation in the field, looking at prominent contenders and game-changing innovations, but also, their short comings for use cases in enterprise settings. We present our solution and close with a call to the community for feedback, and to join this Open Source effort, to be lead by the Linux foundation.



### Contents

* I. Design Goals
    * Decentralisation
    * Digital Assets
    * Smart Contracts
* II. Situation
    * Industry Requirements  
        * Privacy
        * Finality
        * Identity
        * Transaction
        * Order
        * Scalability and Performance
        * Productivity  
    * Current Blockchains
    * Limitations of Current Models  
        * Safety
        * Finality
        * Privacy
        * Performance
        * Scalability
        * Challenge of Mathematical Proof
        * Efficiency
        * Micropayments
        * Usability
        * Development Time
        * Deployment
        * Ecology
    * Industry Trends  
        * Secret Transactions
        * Interacting Blockchains
        * Scalability, Performance, Cost
        * Specialized Payment Transactions
        * Complex Decentralised Application
        * On-Chain Digital Asset Issuance
* III. Proposal  
    * Federation
    * Client-Validator Relationship
    * Identity Management
    * Implementation of Digital Assets
    * Transaction Log Chain
    * Zero Knowledge Proofs
    * Scope of Stake
    * Using Traditional Consensus Protocols
    * Atomic Chain Interfaces
    * Asynchronous Calls
    * Established Tool Chain
    * Optimistic Parallelisation
    * Traditional Network Security
    * Hardware Platform Focus
* IV. Conclusion
    * Differences  
        * No Native Coin
        * No Gas
        * Not Public
        * Node Identity
        * Account Identity
        * Censorable
        * Is This Still a Blockchain?
    * Call To the Community
* Acknowledgements
* Notes
* References



## I. Design Goals

The focus of this design for a blockchain for the industry is to support decentralisation, digital assets and smart contracts.


* **Decentralisation**

	Decentralisation in this context means, not having a central source of truth but sharing all state among all nodes in a network and make changes to it in lockstep. Equally, running distributed applications means, running an application roughly at the same time on all nodes and come to a consensus about the result immediately afterwards. This is an extremely powerful, new approach that allowed Bitcoin to defy common believes and defeat the _double spending_ problem [i]. Its application goes way beyond crypto-currency, and just like the internet put physical information carriers out of business, this is a template of disintermediation in commerce.

	We see decentralisation as a ground-breaking advantage of blockchain technology, allowing industry players to communicate and enter into contracts without having to agree and compensate a trusted third party to police their trades, and without the need to create custom specifications to interface their IT landscapes. The same incentive holds for intra-company systems where departments in international organisation need to work together that might be in competition or are not allowed to delegate trust.

	Decentralisation enables digital assets and smart contracts, which share the essential attribute to not be dependent on and not being censorable by a central authority.


* **Digital Assets**

	Digital assets are sets of numbers that represent actual value, tracked on a decentralised ledger. They are in effect numbers that hold bearer value like cash, thanks to decentralisation and basic cryptography. They can _be_ airline miles, fiat currency-backed units of account, securities, deeds, or of any other type. Ideally, e.g. digital securities are issued directly onto the blockchain, allowing trades to clear within a second, rather than within days, and we believe that this will be the standard a decade from now.

	For a transitional period we will see digital assets only _representing_ real world certificates, like a paper deed or a bond coupon, stored elsewhere, which are still the actual, legal values. Over time, the cryptographically secured key set itself will become the legal proof of ownership. The more permeable this distinction will become, the more powerful will smart contracts and digital trading become.

	Digital assets empower smart contracts.


* **Smart Contracts**

	The primary innovation to leverage is the smart contract. Smart contracts are self-executing agreements between parties that have all relevant covenants spelled out in code, and settle automatically, depending on future signatures or trigger events. If so designed, they cannot be revoked, denied or reversed, as decentralised execution removes them from the control of any one party. They are effectively run on every node at the same time and stopping one on one node just takes the node out of the flow of the network state progress.

	Smart contracts will change the way our economy works, include more people and allow for new types of markets and organisations. They are verifiable applications that enforce business rules, between any parties or devices, and can move real money, either by clearing transfers off-chain [d] or settling digital assets on-chain. They can be used e.g. to co-ordinate devices in IoT [24][25] or implement security derivates that have hard coded behaviour; they can automate payments, effect conditional ownership change, or transfer permission to use a physical asset.

	They can even be used to create Decentralised, Autonomous Organisations (DAO): a self-governing, asset holding corporation that can trade and extend services, which are relevant because they can fulfill  independent, third party business functions, like issuing digital assets.



## II. Situation


### Industry Requirements

Our investigation with major industry players has resulted in a comprehensive set of requirements that we understand as essential for the success of a blockchain for the industry.


* **Privacy**

	Transaction data for industry use cases must be private.

	Obvious reasons of competition are precluding the deployment of current blockchain technology in most business scenarios, which e.g. allows everyone to see every transaction, even though sender and recipient are pseudonymised [c]. 

	Anti-trust regulations are the strongest factor in why privacy has to be stronger for some industrial applications, especially in fintech, than in the current public blockchain implementations. Businesses are not allowed to share as much information with each other as those systems require.

	Thus currently, observable transaction patterns are too easy to interpret to be acceptable and could e.g. give away details about a supplier relationship that cannot be revealed.


* **Finality**

	Data stores for industrial use must provide finality, meaning simply that there must be certainty at what point a value is committed and that after this point, it will not change on its own.

	This cannot be guaranteed by the blockchains like Bitcoin and Ethereum, which offer only incremental likeliness of finality over time [1][26], akin eventual consistency [], rather than a firm promise of finality, and can suffer arbitrary loss of committed information as they are designed to allow for, and cope with, forks in their global state. 

	Enterprise blockchains will instead require immediate and guaranteed finality, based on peer-reviewed protocols.


* **Identity**

	Many business cases require for identities to be established. Apart from often being required by regulations this also allows for reversal of criminal or erroneous transactions, not as built-in mechanism, but by incentive to preserve business reputation and legal recourse.

	Thus, strong identities of both validators, account holders and assets have to be established and managed. 


* **Auditability**

	Proof of past records are a natural strength of blockchains, as a side-effect of how consensus is maintained. But auditability for businesses is a requirement going beyond timestamping a state snapshot and includes the identities of actors, which a public blockchain by design hides behind pseudonyms.


* **Transaction Order**

	For industry use, it has to be ensured that all transactions are treated equally and the _selfish miner_ problem must be addressed: in Bitcoin or Ethereum, miners can freely decide which transactions to include in what order into a block, or to even not include them at all.


* **Scalability and Performance**

	Requirements for scalability and performance vary for industry applications, and a private network does not always have to provide for the expectable load of global all-access use that public blockchains are facing. 

	Providing sub-second latency for transactions and tens of thousands of transactions per second is not currently what Bitcoin or Ethereum offer [59] but is often required in real world business use cases. The Visa network, for example, is allegedly capable of processing up to 56.000 transactions per second [84]. 

	The more convincingly scalability and performance can be addressed, the broader will the applicability of such a blockchain for the industry be.


* **Productivity**

	Development of decentralised applications and smart contracts is a field where many of the traditional rules of software development hold. But new paths have to be beaten for debugging, testing and maintaining applications in a distributed setting, requiring a holistic approach to the creation of the development tools.


* **Experimentation**

	Especially a new technology like blockchain needs a secure but realistic ways for experimentation, to mount proof-of-concept implementations, and the ability to set up a test environment fast, in private and separated from live business.


### Current Public Blockchains

Blockchain technology is the result of decades of cryptology research [30]-[37], mainly from the late 1990s, and today’s implementation of Bitcoin would not have worked on commodity hardware of that day. With Bitcoin as the current most visible success, we are still in the early stages of the field’s development and intensive research efforts are on-going to solve the challenges, e.g. of scalability and privacy.

Bitcoin solved, first of all, the double spending problem [i] without using centralisation, an invention for which there is much wider application in commerce, and beyond, than the special case of a crypto-currency, which Ethereum is setting out to demonstrate. Bitcoin also pioneered implementing a virtual machine to execute transactions [38], which are realised as short scripts, even for the standard case of moving digital currency between two accounts [39]. Ethereum elevated this principle to the next level and added state and Turing-completeness to the capabilities of these transaction scripts, and also gave them a Javascript-like high level language compiler [43][72]. This added fundamental challenges like addressing the Halting Problem [].

To ensure robustness, progress with Bitcoin’s core technology has almost stopped. It is this void into which upstarts like Ethereum or Blockstream [29] venture to propose a ‘Bitcoin 2.0’. But it is also illuminating to look at the first products emerging on top of the individual chains, especially Ethereum, and to understand the challenges they had to overcome on their way.

The desirable features that blockchains like Bitcoin and Ethereum offer are:

* decentralized
    * un-censorable  
    * tamper-proof  
    * unstoppable  
    * ultra robust  
    * very low maintenance  
    * low fees  

* public
    * permissionless  
    * elastic [e]  
    * highly available [f][78]  

* scriptable
    * self-executable  
    * multi-purpose  

* monetary
    * value-holding  
    * crowd funded [g]
    * tradable  

* protected
    * digitally signed  
    * pseudonymous  

In aggregate, this enables hard to stop, self-executing smart contracts for everyone. 

Smart contracts have been described by digital-currency pioneer Nick Szabo in 1997 [40] and their power has started to be realised since the Bitcoin boom of 2013. Their full potential to revolutionise world commerce, and to change our social fabric, has not been widely acknowledged yet. First commercial offerings built around smart contracts use the Bitcoin or Ethereum blockchain today and are only just scratching the surface. 

Bitcoin allows for very simple forms of smart contracts [41] in the context of financial transactions. Ethereum has been conceived on the notion of a generalised meta blockchain, massively enhancing Bitcoin’s scripting capabilities [9][42][43] and thereby providing a platform for a powerful new class of smart contracts [72]. 

The trajectory of Ethereum’s inception was that of freely customisable _financial transactions_ though, and restrictions in its current implementation, to be righted in future releases, are an early legacy of that origin [44].

Less important for the application for the industry are the design goals of Bitcoin and Ethereum to be free-for-all and un-censorable, and this renders many technical sacrifices made to attain this goal – among them privacy and scalability – less palatable in the context of industry applications. There is also no safety measure against _selfish mining_ with the current blockchain designs, where a miner creates blocks that consist only of transactions she favours and delays others.


### Limitations of Current Models

Bitcoin, Ethereum and similar blockchain protocols were designed for a different requirement set than serving industry use cases. The concessions the Bitcoin approach makes, and which Ethereum inherits, to be at the same time decentralised and immune to double-spending exploits, are numerous. They include safety, finality, privacy, funds lock-in, performance, scalability, efficiency, micropayments, maintenance, usability and ecology. Not a concession, but a powerful design decision is non-reversability. Also, neither Bitcoin nor Ethereum were made for setting up instances of new, private chains; and the community–driven and crowd–funded approaches have lead to an uneven maturity in the available tools.

As per the industry requirements listed above, some limitations, like privacy and finality, are intolerable. But in the same breadth – because we have different design goals, notably among them, a private network – most concessions turn out to be unnecessary for an industry blockchain.


* **Safety**

	In current public blockchains, safety is compromised in that any open blockchain is susceptible to _Sybil attacks_ [73] where an attacker creates so many nodes on it, or gains control over so many nodes that he can use this majority to disrupt communication within the network, cutting other nodes out from it; or gain a share of the hashing power high enough to undo transactions, as a stepping stone towards double spending [i].

	The issue is moot for permissioned networks where strong identity is established, so no-one can operate multiple nodes, and where out-of-band incentives exist, of legal, repetitional or monetary nature, to make criminal behaviour unattractive.


* **Finality**

	Finality is sacrificed in the sense that any truth might be replaced by a different truth without warning, when there is a collision in the random leader election, or a technical problem with the network, or a program error. This can result into ‘forks’ [74][1], where two different views of valid state exist in the network at the same time, violating the expectation of consistency.

	A ‘fork’ is an acceptable state for Bitcoin or Ethereum and both use a similar, simple mechanism to decide which of the two or more forks should be retained [27][28] and which fork, including its state diff, should be dropped.

	The result is a partial reset of the state in the nodes on the loosing fork, which usually invalidates transactions, without good predictability and no recourse. A payment might have looked perfectly good for some time, and goods been shipped accordingly, only for the payment to disappear when the fork is abandoned.

	The Bitcoin and Ethereum blockchains are designed to detect and manage forks, so that forks exist as briefly as possible, which makes finality of any information more likely the longer that information is part of the known truth of a participant node. But while forks last they cannot reliably be detected by the system [75], they can be the result of rare bugs [76] and both the Bitcoin mainnet [77] and the current beta version of Ethereum [] have experienced an hours-long lasting fork, each.

	Our proposal provides a guarantee of finality at the low cost of lesser availability [78] in exceptional cases.


* **Privacy**

	Privacy is sacrificed because all interested participants double–check all proposals by the leaders who propose a new truth, by doing the exact same calculations that the leader proposes and therefore, all nodes can deduce all facts of that new truth.

	We address this challenge proposing _scope-of-stake_ and preparing for the advent of less resource-greedy zero-knowledge solutions.


* **Performance**

	Both Bitcoin and Ethereum have strong limitations with performance that do not compare favourably at all with centralised approaches [k]. The synchronisation of all interested participants to a new truth includes a math competition, called _proof-of-work_ [l], to determine which node may propose a new block. The competition dents performance very severely and can be sped up only so far before its protective purpose is defeated.

	This is a mechanism that makes sense for public networks but can be abandoned for private networks, which can choose leaders in a predictable sequence without risk.


* **Scalability**

	Scalability was not an initial design goal of Bitcoin, and Ethereum 1.0 inherits some of the limitations. The Bitcoin mainnet blockchain is basically the giant log of all Bitcoin transactions ever. It would fast become unwieldy if Bitcoin experienced serious mainstream adoption.

	In Ethereum, knowing the latest block is sufficient to be able to verify the entire state, which is not part of the blockchain proper. But clients of the 1.0 implementation are not optimised and keep the entire state that ever existed.

	Other offerings that proclaim much better parameters in this respect are usually centralised solutions that implement only some aspects of blockchain technology and sacrifice others to achieve high throughput [].

	Research to overcome the scalability challenges has high priority in the Bitcoin and Ethereum community, and is subject of academic work. As a fundamental challenge, its solution is understood to have to be anchored in the most basic design level, like Ethereum’s state trees demonstrate [79].

	We propose reducing transaction validation to those parties who have a stake in them and a transaction responsibility scheme that allows proposers to work in parallel, complemented by a scheduling mechanism that identifies, which transactions can be parallelised to be able to leverage parallel computing power in nodes.


* **Challenge of Mathematical Proof**

	Neither Bitcoin, Ripple nor Ethereum have been proven to be mathematically correct. Bitcoin and Ripple can claim to have been battle–hardened in real world use but going forward, more elaborate adversaries might find fatal flaws in their protocols that have just not been discovered yet, as the target was not valuable enough yet. Transaction malleability [69], ASIC-mining [70] and selfish mining are some of the flaws understood now about the Bitcoin design and there is no proof that all problems have been found.

	We will prove our proposed design mathematically before releasing it, to be able to give assurance of safety.


* **Efficiency**

	Bitcoin and Ethereum are jokingly called “the most ineffective way to compute”. As every node in the network currently computes everything that anyone on the network triggers to be computed, they are in some sense maximally wasteful and slow – while at the same time in many ways also the most economic and fastest solution in their domain.

	The sharding that we propose greatly reduces this waste and the incurred loss of performance.


* **Funds Lock-In**

	Public blockchains are well suited to codify insurances or investments funds but the nature of smart contracts on the Bitcoin blockchain requires that all funds of future payments are locked-into the contract account, while in Ethereum contracts might turn out worthless when they are implemented without a lock-in.

	This is a problem that native blockchain tokens cannot overcome but implementing digital assets as smart contracts opens the door to leverage locked-in value.
	

* **Micropayments**

	Although Bitcoin miners still receive a block reward, their additional earnings from transaction fees are at USD 5 Cents per transaction, making Bitcoin unsuitable for micro transactions on the blockchain. There are several approaches to this challenge but they would require extensions to the Bitcoin protocol [].

	While the current transaction fees might be a consequence of unexpectedly high valuation of Bitcoin, they are linked to the high cost of security in an open network.

	Because micropayments let the blockchain grow rapidly they are also seen by some as less desirable for the Bitcoin ecosystem on the whole. The assumable higher volume of Micropayments, compared to transactions that transfer higher amounts, might overload the network and make it unavailable for every type of transaction.

	As we leave the security overhead of miners and _proof-of-work_ behind, our proposal is suited for a high volume of small transactions, which can be important in IoT.


* **Usability**

	Usability can be seen as hampered by the independent design decision to not allow for reversals, even though this can add much safety for the receiver of a transfer, when handled patiently. 

	Technically, Bitcoin transactions complete magnitudes faster, within minutes or hours, than credit card transactions that are reversible for 60 to 90 days. But as a consequence of slow blocktimes, paying with Bitcoin in practice can be a much slower experience, 10 minutes to hours, than paying by credit card, which takes 3 to 60 seconds to receive a (reversible) ‘confirmation’. Or, it is less safe when a merchant decides to be content with ‘zero confirmation’ [], i.e. waiting only for a transaction to be broadcast, within around 20 seconds, rather than waiting for it to be included in the blockchain.

	Ethereum solves this by running much faster blocktimes, at around 10 seconds.

	But both blockchains allow for forks as a design principle, which can lead to confirmed transactions disappearing without recourse, so they cannot give an actual guarantee of completeness, at any point in time. 

	Our proposal allows for finality in under a second.


* **Maintenance**

	There is no upgrade path for either the system nor the smart contracts running on a blockchain. Due to their anonymous and decentralised nature, for Bitcoin and Ethereum themselves, only voluntary software upgrades can be suggested to miners and a hard fork can have success if it benefits the miners. Due to the design-goal of uncensorability Smart Contracts do not have native upgrade path but can be programmed so that they can be replaced by a successor in a three-pronged strategy.

	Hot system upgrades can be performed in a private industry blockchain as a matter of business agreement, the benefactors here will also always be the node operators, who are intrinsically incentivised to optimise the network. As envisioned but realised by Ethereum, we will introduce a name-service to give smart contracts a upgrade-path.


* **Development Time**

	Developing proper smart contracts in Bitcoin is almost impossible beyond a very narrow set of crypto-currency use-cases [71]. Ethereum allows for a broad array of applications and the implementation of complex mechanisms [72]. However, some inherent design choices that originated from a bottom-up approach, demand idiosyncratic layouts to succeed.

	Our proposal, from the onset, is made to allow for a productive development system and hosted, private blockchains as a service.


* **Deployment**

	The bootstrapping process of both Bitcoin and Ethereum was partially manual to get the global main chains started in a decentralised fashion. Provisions to help starting private chains are not a focus of development in either project.

	Our design reflects the requirement to get a new chain up and running, by a developer without prior experience, in under an hour.


* **Ecology**

	Bitcoin miners use electricity equivalent to what Ireland uses over the year, to the tune of $300.000.000. This is the price to secure the open Bitcoin mainnet against attacks, it is the power that is consumed to run the math competition needed to propose a block.

	We do away with this cost entirely as our proposal envisions interested, identified validators who own the network.



### Industry Trends

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




## III. Proposal


We propose the following design for a blockchain for the industry as solution to the requirements we found that cannot be aligned with the limitations of current blockchain impplementations. Like with the original Bitcoin design, we are leveraging existing approaches, for an emergent sum of parts, aiming at a different purpose.


* **Federation**

	Validators, who verify [m] and sign blocks of transactions, extend a protected, private network that hosts a permissioned [n], distributed [o] ledger and has a centralised entry provision process not part of the core blockchain implementation. The proposer of each new block is chosen from the validators.

	The validators have a real world, legal relationship with each other, e.g. banks taking part in a consortium that operates the network.


* **Client-Validator Relationship**

	Transactors technically access the system through one of the validators’ nodes and legally have a business relationship with the validator, e.g. bank customers going through an entry point provided by their bank to transact over an inter-banking network.

	This allows for a new privacy concept for transactions and to ensure that transactions will be ordered fairly.


* **Identity Management**

	Strong identity management of accounts is provided that allows to prove businesses that they know who did what using their network, to satisfy regulations like AML/KYC.

	A centralised component is introduced to establish the reliable link between real world identities and blockchain accounts.


* **Implementation of Digital Assets**

	Digital assets are implemented as smart contracts on the blockchain, and there is no native crypto-currency blockchain token, like Bitcoins, Ether [p] or XRP.

	This allows to easily create new asset types and for any account to hold any number of different digital asset types as opposed to only one count of the native crypto-currency.


* **Transaction Log Chain**

	To allow for higher privacy protection, the Bitcoin approach of storing transactions as substance of the blockchain, is preferable over the persisting of balances that Ethereum implements [80].

	This concept of unspent transactions outputs, UTXO [], is more generic and powerful, a logical equivalent of how SQL databases are transaction logs at heart, rather than values stored in tables and columns, which are but a useful abstraction.


* **Zero Knowledge Proofs**

	_Zero-knowledge proofs_ [50] are a cryptographic technique to provide a proof that one owns a certain piece of information, without revealing it. The proof usually entails using the knowledge to answer a set of random test questions. This can be used e.g. to prove that one has the right to spend a digital asset.

	As soon as they become usable, zero-knowledge, succinct non-interactive arguments of knowledge (zk-SNARKS) [47] will be applied to secure transaction contents and account ownership. Currently, size and computation time are a hurdle to adoption with a required gigabyte-large initial database for verifying, and a minute-long processing effort to produce a proof [].


* **Scope of Stake**

	We propose that every validator can only see in clear text what they need to see, for every given transaction, with visibility levels being associated with state as needed to be handled by the smart contracts. This leverages the fact that in industry applications, every account will on the business level usually be associated with exactly one validator (bank/bank account).

	As a consequence, a block’s content is known only to involved parties, who are also the only ones verifying and signing off on it. E.g. a bank from which the transaction is originating, the bank that has the receiving account, and an auditor’s node. The other nodes do not share the knowledge but accept the block, in encrypted form, signing off only on the hash itself and its timestamp, fundamentally unconcerned about the content of the specific block as it contains no accounts associated with them.

	The blockchain then becomes a variably encrypted ledger shared between intersecting groups of validators, plus common state, transactions and commonly used smart contracts, shared by all participating nodes. 

	This encryption method provides complete privacy of transactions in regard to sender and receiver, amount and asset type, at the cost of less sweeping validation. Different from zero-knowledge proofs, this comes with no performance penalties but paves the way towards parallel processing.


* **Using Traditional Consensus Protocols**

	We use a traditional consensus mechanism, a peer-reviewed byzantine fault tolerant protocol [81], developed for distributed computing, instead of a forking protocol, to allow for immediate finality, sub-second block times and transaction speed, as well as throughput of thousands of transactions per second.

	Accepting forks was a concession of public blockchains to allow for an elastic network, open for everyone and with anonymous participants, but is neither tenable nor necessary for industry applications, and we don’t follow the reasoning that the responsibility to work with eventual consistency should be pushed on the application developers, as this opens a Pandora’s box of severe, very hard to detect error sources []. 

	Using a traditional consensus protocol will avoid any loss of state at the low cost of reduced availability in extreme cases, where processing will halt once too many nodes of the network become unavailable. However, these attack scenarios are not expected in a protected network, which uses a centralised list of allowed nodes and is firewalled-off from public access. Unlike in a public network, these defences would have to be breached first to then allow an attacker to take out more than a third of nodes and by this stop the network.


* **Atomic Chain Interfaces**

	Atomic communication between different chains enables secure value exchange between them and a more robust and specialised ecosystem of blockchains that serve fundamentally different purposes. Bitcoin payments or access to public ledgers implemented on the global Ethereum chain will be useful functions of an industry blockchain.


* **Asynchronous Calls**

	Distributed computing is asynchronous in nature and predictably, the development of smart contracts turns out to be forced to respect this. The calls between smart contracts should be uncoupled by default and an asynchronous message passing mechanism added that allows for a completion of a processing step while a desired result is calculated by another contract.

	Computations that require multi-step state persistence can thus be completed faster and programming becomes more in-tune with the actual order of execution.


* **Established Tool Chain**

	A well known language and an established compiler and virtual machine are to be used for the execution of contract code stored on-chain.

	This will yield better performance and allow to leverage existing libraries, developer tools and know-how.


* **Optimistic Parallelisation**

	State management is to be implemented in such a way that it allows for optimistic parallel execution of smart contracts, focussing on identifying commutative and idempotent attributes of contracts.

	Research and good experience exists in this regard with the implementations of high-performance database clusters [82] and this will open the path to utilise the power of multi-core processors, as well as potentially scale individual validators horizontally.


* **Traditional Network Security**

	Traditional network security is applied to secure the privacy and integrity of the network. This replaces the overhead of miners who extend their hashing power to secure an open network [q]; and it serves as first perimeter to ensure data protection.

	Firewalls, and technical separation from publicly accessible networks, as well as optionally physically separate hardware will be employed to create a secure environment. It is of note that the blockchain protocol further strengthens these defences by securing the ledger against attacks even if an attacker manages to penetrate the network. More than a third of validator nodes would have to be taken out to stop the network, and at least two thirds broken into to insert false data.


* **Hardware Platform Focus**

	For a wide array of applications, e.g. in fintech, we can make an assumption that validators will run high–end server instances with industrial bandwidth and high–end processors, capable of significant parallel computation, which should be leveraged to achieve maximal performance.

	This also allows for accepting a perceived shortcoming of the Bitcoin blockchain design, the steady increase of the blockchain database size. Even though forced pruning can be implemented straight forward, it is in the interest of performance to provide for ample time before this garbage collection process has to be triggered.




## IV. Conclusion

### Differences

The proposed solution differs in a number of ways from previous designs.


* **No Native Coin**

	Our proposal does not require a native crypto-currency coin, neither for crowd-funding purposes, nor the transaction of digital assets, or enforcement of fairness in the use of computational resources.

	Instead, digital assets are implemented as smart contracts on the chain.


* **No Gas**

	With known validator identities and target use-cases of generally more limited numbers of validators, we will not need a concept like Ethereum’s gas to ensure fairness in the use of computing power and storage resources.

	A hard limitation of both to avoid damage from bugs and sabotage will suffice and keep the execution of smart contracts free from a significant performance burden, as counting gas comes at a cost at every single operation of smart contract execution.


* **Not Public**

	As per its requirements, this proposal is not for open and un-censorable networks that anyone can join as account holder or validator. To the contrary, we introduce strong identities and anticipate a well protected network that only validators have direct access to. Even though, depending on the implemented business case, everyone might be able to join as a transactor in seconds.

	This makes obsolete the provisions that are present, e.g. in Bitcoin and Ethereum to allow for an open network, chief among them maximal availability and elasticity, as well as DDOS-safe leader election [r] and quasi anonymity for validators (miners) and account holders (key owners). 


* **Node Identity**

	Business requirements will see verified identities of all nodes in the private blockchain described here. Especially validators – the corollary to miners in the Bitcoin world – will be not anonymous but vested and official participants, running the network together, e.g. based on consortium business decisions.


* **Account Identity**

	Many use-cases we investigated point to strong legal requirements to know who a business is transacting for. As a consequence, we will have strong verification of owners of accounts, delegated to the validators that an account is associated with.

	The individual accounts on the blockchain however, will be much more protected than on public blockchains, and unidentifiable. Because the network infrastructure will not be public but firewalled off, the ledger will not be readable by any unauthorised party.


* **Censorable**

	With networks private and a desire to meet legal requirements and regulations by all participants, it becomes irrelevant to be uncensorable. 


#### Is This Still a Blockchain?

The proposal is a blockchain in the sense of being decentralised, though only in operation.

Strong identity management and a known set of validators add centralised components, abandoning the uncensorable quality of public blockchains like Bitcoin and Ethereum.

The main goal of enabling smart contracts, with full settling capabilities, strong protection against tampering and auditability [s] is achieved.



### Call To the Community

We believe that blockchain technology is nothing short of revolutionary and will democratise and transform commerce, in the way highways or the advent of the internet did.

Join us to create the most powerful interplay between smart contracts and digital assets, set up in the new, distributed way that has put many established wisdoms of database science on its head.

For digital assets to be secure, they need to be issued into a proven and hardened blockchain infrastructure, made for purpose rather than adapted as good as possible. Safety and finality guarantee will drive the adaption of the practice to issue e.g. securities or deeds directly on the blockchain.

We are starting with a clear premise, rejecting sacrifices that we don’t need to make and embracing the reality of the enterprise domain, like high-powered hardware, industrial network protection, identification by name and address of all participants, and the essential pattern of the customer relationship between transactors and validators.

We make strong choices, like sticking with the Bitcoin transaction model and replacing a ground-breaking innovation of blockchains – the fork-choice rule – with a better suited protocol, to achieve private transactions with guaranteed consistency.

We will need to draw from all these assets to succeed in building a new blockchain that allows for the performance and scale that big markets and complex business logic require.

This will make this platform the technology of choice for business consortiums and industry groups to stand up new exchanges, trade routes and channels that serve provably correct, up to date information.

Your feedback and participation in this Open Source effort, to be lead by the Linux Foundation, is highly appreciated.



## Acknowledgements

We are indebted to Joe Latone of the IBM Lab Almaden, Vitalik Buterin, Gavin Wood, Vlad Zamfir and Martin Besce of Ethereum, and Dominique Williams of Pebble, for education in the field. Gustav Simonsson of Ethereum pointed out errors and needed clarifications in this paper’s draft. IBM Fellow John Cohn relentlessly enforced concreteness, focus and spirit, Martin Kienzle of IBM Research helped giving important topics the right space.



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

[23] Linux Foundation official site – []

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



## Author

Henning Diedrich, hdiedrich@de.ibm.com



## Changes

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


