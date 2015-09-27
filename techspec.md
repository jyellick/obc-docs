# Openchain Technical Specification

_This document is subject to change as development progresses_


## Background

This paper describes the principles, high-level architecture and initial technical specifications of a blockchain suitable for industrial use cases. With Bitcoin popularizing the domain, awareness since 2009 has now reached the point that demand for a solution suitable for industry is surging.

The design presented here describes a blockchain fabric, a protocol for business-to-business and business-to-customer transactions. It is intended for permissioned networks, and it allows compliance with regulations and respects the requirements that arise when competing businesses work together on the same network. We describe permissioned networks as ones where validating and non-validating nodes are run by known whitelisted organizations and where transactors on the network receive identity from an issuing authority service on the network. Depending on the purpose of the network, the issuing authority can make it very easy to get an identity and transact (like getting a Gmail account) or very restrictive. A network can run very publicly, making it easy, for example, to integrate into a mobile app project. Or it can be completely private and known only to parties that have been invited and whose identity has been validated. Because the fabric is designed to support many networks for many different purposes, and to allow addressing between them, the protocol must allow for these different kinds of uses and different levels of permissioning. But the protocol is not designed to allow the creation of a network of anonymous validators or miners operating outside regulatory oversight. 

The central elements of this specification are smart contracts (we call it chaincode), digital assets, record repository, and a decentralized network and cryptographic security. To these blockchain staples, we add performance, verified identities, private transactions, and modular consensus protocols.

####Why a new fabric:
Blockchain technology is in its infancy and is often not well suited for the needs of industry. Scalability challenges and the lack of support for confidential contracts and private transactions, among other issues, make its use infeasible for many important industry applications. We lay out an industry–focused design, based on and extending the learnings of the pioneers in this field.

####Proposed:
In this paper, we describe what we believe are the technical features and initial priorities of a new industrial blockchain design. We list what we have found to be key requirements for industry use, based on experience with industry proofs of concept and on review of existing fabrics and innovations. 

####Core principles:
The focus of this design is to support decentralization (spreading the validation function across many whitelisted legal entities, but not to the point of including anonymous parties), digital assets, and logic (validated, immutable, executable instructions).

####Your voice here:
If you are reading this, then it means anything said in this document or done in this open source repository is open for your input. Apply your experience, lend your voice, and show with your code how the specification and the implementation of Openchain should be taken forward. Don’t like something you see here? Help us all see your point of view (in a respectful and positive way) or even better, bring some code that shows a better way.

##Design Goals

####Decentralization
Decentralization in this context means sharing state among all nodes in a network and making changes to it in lockstep. Equally, running distributed applications means running an application roughly at the same time on all validation nodes while coming to a consensus about the result immediately afterwards. 
Decentralization allows industry players to communicate and enter into contracts without the need to create custom specifications to interface their IT landscapes. 

####Digital Assets
Digital assets are sets of numbers that represent actual value, tracked on a decentralized ledger. They are in effect numbers that hold bearer value like cash, thanks to decentralization and basic private/public key cryptography. They can be airline miles, fiat currency-backed units of account, securities, deeds, or of any other type. 

####Logic
Blockchain logic, often referred to as “smart contracts,” are self-executing agreements between parties that have all relevant covenants spelled out in code, settle automatically, and can be dependent upon future signatures or trigger events. In the Openchain project, we call this “chaincode” to help hold clarity between blockchain logic and the human-written contracts that they can sometimes represent.  Blockchain logic is embodied in verifiable applications that enforce business rules between any parties or devices and can move real money, either by clearing transfers off-chain or settling digital assets on-chain. They can be used to co-ordinate devices in IoT or implement security derivatives with hard coded behavior; they can automate payments, effect conditional ownership change, or transfer permission to use a physical asset.


## Industry Use Cases

We have compiled a set of initial blockchain requirements that are considered essential for supporting the following abstract use cases. (Note: use cases here help guide architecture and test-driven development. While still a work in progress, the use cases should be something all contributors agree on...both in the content and stack-ranked prioritization of them. Propose changes if you feel these miss the mark. It is best if there are no more than 4 abstract use cases, and three is preferred.):

#### B2B Contract

Business contracts can be codified to allow two or more parties to automate contractual agreements in a trusted way.  Although information on blockchain is naturally “public”, B2B contracts may require privacy control to protect sensitive business information from being disclosed to outside parties that also have access to the ledger. 

While confidential agreements are a key business case, there are many scenarios where contracts can and should be easily discoverable by all parties on a ledger. For example, a ledger used to create offers (asks) seeking for bids. This type of contract may need to be standardized so that bidders can easily find them, effectively creating an electronic trading platform with smart contracts (aka chaincode).

#### Manufacturing Supply Chain

Final assemblers, such as car manufacturers, can create a supply chain network managed by its peers and suppliers so that a final assembler can better manage its suppliers and be more responsive to events that would require vehicle recalls (possibly triggered by faulty parts supplied by some supplier). The blockchain fabric must provide a standard protocol to allow every participant on a supplychain network to input and track parts produced and used on a given vehicle.

Why is this specific example an abstract use case? Because while all blockchain cases store immutable information, and some add the need for transfer of assets between parties, this case emphasizes the need to provide deep searchability back as many as 5-10 transaction layers. It is the core of establishing provenance of any manufactured good that is made up of other goods and supplies.

#### Asset Depository 

Assets such as financial securities must be able to be dematerialized on a blockchain network so that all stakeholders of an asset type will have direct access to that asset, allowing them to initiate trades and acquire information on an asset without going through layers of intermeidaries. Trades should be settled in near real time and all stakeholders must be able to access asset information in near real time. A stakeholder should be able to add business rules on any given asset type, further reducing operating cost with automation logic.

_For more details about these use cases and their requirements, click [here] (usecases.md)_

## Challenges

Openchain will be built to enhance the blockchain protocol with enterprise features for networks, where validators are whitelisted, known parties who set whether transactors are private or public. The protocol must address the following key challenges:

####Private Transactions and Confidential Contracts
Currently, transaction patterns are too easy to be observed and interpreted. Shared ledgers may give away details about a supplier relationship that should not be revealed to their competitors. In tight supplier/buyer communities, even one party's relative volume of trade is something which would not be appropriate for a system supporting trade between parties to reveal. Therefore, a business ready blockchain must provide mechanisms to conceal identity, transaction patterns, and terms of confidential contracts from being publically identified by unauthorized third parties.

####Identity and Auditability
While private transactions are important, business usage of blockchain also needs to comply with regulations and make it easy for regulators to investigate transaction records. Also, a party must be able to prove its identity and ownership of an asset after the fact, perhaps years after the fact, without the mechanism for establishing that identity being able to be used by bad actors to appropriate a party's identity or ascertain their activities on the ledger. 

####Finality
Data stores for industrial use must provide finality, meaning simply that there must be certainty at what point a transaction is committed, and that after this point, it will not change on its own. Many current blockchain networks offer only eventual consistency, rather than a firm promise of finality, and can suffer arbitrary loss of committed information as they are designed to allow for, and cope with, forks in their global state. A business ready blockchain network will instead require immediate and guaranteed finality, based on peer-reviewed and battle-hardened protocols. 

####Transaction Order
For industry use, transactions orders must be more efficiently managed and bring more market fairness to address the miner problems seen in the public blockchain world today, where minors can freely decide the order of transactions to include in the chain based on their own needs.

####Performance and Scalability
The level of performance for enterprise grade blockchain network may be much higher than that of public networks. For example, to allow real time settlement of financial securities, a blockchain network needs handle up to 100,000 transactions per second, with sub-second latency. Both requirements are difficult to achieve with the consensus protocols in use today, but nevertheless they are requirements for a business ready blockchain fabric. 

####100-year Decisions
If blockchain becomes the fabric of an economically aware Internet, then it must be designed for performance over the long haul. A ledger or set of ledgers must be able to operate continuously for 100+ years and still allow discoverability, search, identity resolution and other key functions in user-acceptable timeframes.

####Productivity
Development of decentralised applications and smart contracts is a field where many of the traditional rules of software development hold. But new paths have to be beaten for debugging, testing and maintaining applications in a distributed setting, requiring a holistic approach to the creation of the development tools.


## Design Philosophy

#### Assigning Roles

Beginning with a private network, validating nodes are operated by whitelisted members responsible for maintaining the ledger (via configuration). They are the only entities that can verify, process, and commit transactions to the ledger, based on the application of a consensus algorithm (see modular design below). 

Non-validating nodes don’t write to the ledger, but they serve a very essential role in the network: Taking the workload off the validators in servicing client requests.

#### Modular Consensus 

Consensus algorithms on the protocol are pluggable, allowing users to select the consensus algorithm of their choice during deployment. Different networks may deploy different consensus algorithms to fit their usage scenarios. The protocol will provide a Practical Byzantine Fault Tolerance (PBFT) [4] implementation in its initial release, and we expect the community to contribute more consensus algorithm modules later.

#### Tracking & Validating Participants

The protocol starts with an identity, which is a cryptographic certificate encapsulating a user’s confidential data registered on a Registration Authority. The Registration Authority can issue and revoke identities participating in a network. From this identity, the protocol can generate butterfly keys [3] for members to transact on a network, and these keys will conceal the identities of the transacting parties, providing privacy support to the network.

#### Protecting Confidentiality

Content confidentiality is achieved by encrypting the transactions such that only the stakeholders can decrypt and execute them. In addition, a piece of business logic (chaincode, aka smart-contract) can also be cryptographically secured (if its confidentiality is required by its stakeholders) and will only get loaded and decrypted at runtime. 

#### Running Business Logic

The protocol specifies 2 types of transactions: code-deploying transactions and code-invoking transactions. Business logic that is deployed and invoked on a ledger network is called chaincode. A code-deploying transaction may submit, update, or terminate a piece of chaincode, and it is the validators’ responsibility to protect the authenticity and integrity of the code and its execution environment. On the other hand, a code-invoking transaction is an API call to a chaincode function. This process is similar to how URI invokes a servlet in JEE. Note that each chaincode maintains its own state, and a function call is a common way to trigger chaincode state changes.

The chaincode concept is more general than the smart contract concept defined by Nick Szabo [4]. Chaincode can be written in any mainstream programming language and are executed in Docker containers inside the Openchain context layer. Chaincode provides the capability to define smart contract templating language (similar to Velocity or Jade) and to restrict the functionality of the execution environment and the degree of computing flexibility to satisfy the legal contractual requirements. 

Chaincode transactions are time bounded and configured during chaincode deployment. This is similar to a database call or a Web service invocation. If a transaction times out, it is considered an error and will not cause state changes on the ledger. One chaincode function may call another chaincode function if the callee has the same or less restrictive confidentiality scope; that is, a confidential chaincode may call a public chaincode or another confidential chaincode if they share the same group of validators.

To meet the confidentiality requirement required by some business agreements written in chaincode, appropriate validators must get assigned before deployment. This will create quorums of validators during execution of transaction blocks. Validation nodes not selected to validate a confidential chaincode can just request for the state updates from those who are. At the end of each successful block execution consensus, the world state must be consistent on all validating nodes.


#### Accessing Openchain
Openchain includes REST and JSON RPC APIs, events, and an SDK for applications to communicate with Openchain. Typically applications interact with a peer node, which will require some form of authentication to ensure the entity has proper privilege to interact with the network. Messages from a client are signed by the client identity and verified by the peer node.
Note that the Openchain fabric does not include any native crypto-currency like Bitcoins or Ether. However, users can be easily implement them with chaincode.


## Architecture
Figure 1 below shows the reference architecture aligned in 3 categories: Membership, Blockchain, and Chaincode. These catagories are logical structure, not a physical depiction of partitioning of components into separate processes, address spaces or (virtual) machines.

Some of these components will be built from ground up; some will use existing open source code as appropriate, and some will just interface with existing services to fulfill the required functions.

![Reference architecture](refarch.png)
Figure 1:  Openchain Reference architecture

The Membership category provides services for managing identity, privacy, and confidentialy on the network. Participants register to get identities, which will enable the Attribute Authority to issue butterfly keys to transact. Reputation Manager enables auditors to see transacations pertaining to a participant. Of course, auditors will have to be granted proper access authority by the participants.

Blockchain services manage the distributed ledger through a peer-to-peer protocol, built on HTTP/2. The data structures are highly optimized to provide the most efficient hash algorithm for maintaining the world state replication. Different consensus (PBFT, Raft, PoW, PoS) may be plugged in and configured per deployment.

Chaincode services are a secured and lightweight way to sandbox the chaincode execution on the validators. The environment is a “locked down” and secured container along with a set of signed base images containing secure OS and chaincode language, runtime and SDK images for Golang, Java, and Node.js. Other languages can be enabled if required.


#### MEMBERSHIP

<table border="0">
<col>
<col>
<tr>
<td width="30%"><img src="refarch-memb.png"></td>
<td valign="top">
Openchain is a private-validator network protocol, so all entities are required to register with membership services to get identity to access and to transact on the network. Validators at network setup can determine the level of permissioning required to transact. It is possible for a network to be very liberally permissioned, allowing ease of access supporting rapid and high adoption goals, and it is possible for a network to be very restricted. The validators that set up the network configure this.<p><p>

Registration Authority issues enrollment certificate necessary to establish identity for a member. Once registered, the member has the required credentials to participate in the network, no need for proof-of-work or proof-of-stake at this point. <p><p>

Attributes Authority issues butterfly keys to members to transact and to ensure the privacy of the members on the network. Non-stakeholders can't link the transactions back to the members. Transactions appear to be coming from random addresses, completely shuffled by the system. <p><p>

Reputation Manager allows authorized auditors to link butterfly keys to identity, and consequently, prove the relationship between transactions and members.
</td>
</tr>
</table>


#### BLOCKCHAIN

<table border="0">
<col>
<col>
<tr>
<td width="50%"><img src="refarch-block.png"></td>
<td valign="top">
Blockchain services consists of 3 key components: Distributed Ledger, Consensus Manager, and Peer-to-Peer (P2P) Protocol. <p><p>

P2P Protocol uses <a href="http://www.grpc.io/docs">Google RPC</a>, which is implemented over HTTP/2 standards, providing many capabilities including bidirectional streaming, flow control, multiplexing requests over a single connection. And most important of all, it works with existing Internet infrastructure – firewalls, proxies, and security. This component defines messages used by peer nodes, from point-to-point to multicast. <p><p>

The Distributed  Ledger component manages the blockchain and the world state with 3 key attributes:
<ul>
  <li>Efficiently calculating a cryptographic hash of the entire dataset after each block</li>
  <li>Efficiently transmitting a minimal "delta" of changes to the dataset, when a peer is out of sync and needs to "catch up”</li>
  <li>Minimizing the amount of data kept around necessary for each peer to operate</li>
</ul>
</td>
</tr>
</table>

The Ledger uses <a href="http://rocksdb.org"> RocksDB</a> to persist the dataset and builds an internal data structure to represent the state that satisfies the above 3 criteria. Large documents or files are not stored on the Ledger but off-chain storage. Their hashes may be stored on-chain as part of the transactions. This is necessary to maintain the integrity of the documents or files.

The world state represents the state for all chaincodes. Each chaincode is assigned its own state that can be used to store data in a key-value format, where keys and values are arbitrary byte arrays. The world state also contains the block number to which it corresponds.

As transactions are run in a new block, a delta from the world state in the last block on the blockchain is maintained. If consensus is reached for the current block, the changes are committed to the database, and the world state block number is incremented by 1. If peers do not reach consensus, the delta is discarded and the database is not modified.

Consensus Manager is an abstraction defining the interface between the consensus algorithm and the other components. Consensus receives transactions, and depending on the algorithm, decides how to organize the transactions and when to execute the transactions. Successful execution of transactions results in changes to the ledger.

Openchain provides an implementation of Byzantine Agreement with advanced features in fault tolerance and scalability.

Event Hub in a decentralized network is complex in nature, as an event may appear to occur multiple times, each on a peer node. Callbacks can end up receiving multiple invocations for the same event. Therefore, a peer node (preferably non-validator and local) manages the event pub/sub that applications are interacting with. The peer node emits events as conditions satisfied in no particular order. Events are not persisted — fire-and-forget, so applications should capture events if required.



#### CHAINCODE
<table border="0">
<col>
<col>
<tr>
<td width="30%"><img src="refarch-chain.png"></td>
<td valign="top">
As defined in the previous sections, chaincode is a decentralized transactional program, running on the validators. <p><p>

Chaincode Services use <a href="https://github.com/docker/"> Docker </a> to host the chaincode without relying on any particular virtual machine or computer language. Docker provides a secured, lightweight way to sandbox the chaincode execution. The environment is a “locked down” and secured container along with a set of of signed base images containing secure OS and chaincode language, runtime and SDK images for Golang, Java, and Node.js. Other languages can be enabled if required. <p><p>

Secure Registry service enables Secured Docker Registry of base Openchain images and custom images containing chaincodes.

</td>
</tr>
</table>


## Application Programming Interface
![Reference architecture](refarch-api.png) <p>
At the top, CLI is the command line interface to the network. Openchain provides a set of CLIs to administer and manage the network. CLI can also be used during development to test chaincodes. ReST API and SDK are built on top of JSON-RPC API, which is the most complete API layer. SDK will be available in Golang, JavaScript, and Java. Other languages can be added as necessary.

The API spans the following categories:
*  Identity - Enrollment to get certificates or revoking a certificate
*  Address - Target and source of a transaction
*  Transaction - Unit of execution on the ledger
*  Chaincode - Program running on the ledger
*  Blockchain - Content of the ledger
*  Network - Information about the blockchain network
*  Storage - External store for files or documents
*  Event - Sub/pub events on blockchain

### Identity
*  Register: Register a user or endpoint for identity
*  Revoke an Identity: The identity will no long be valid in the network
*  Get Identity from Address: Return the identity owning the address

### Address
*  Get Addresses: Return addresses belongs to an identity
*  Create Address: Create and return a butterfly address for an identity

### Transaction
*  Get Transactions by Address: Return transactions of an address
*  Get Transaction by Hash: Return transaction identified by the hash
*  Get Transactions by Identity: Return transactions belong to identity

### Chaincode
*  Deploy (optional multisig): Deploy a chaincode to the ledger. In the multisig case, the chaincode is only active when all the signatures are present; otherwise, calling the chaincode will result in errors.
*  Undeploy: Mark the chaincode as invalid; call any function return error
*  List: Return all visible chaincodes
*  Run a Function: Call a chaincode function as a transaction
*  Test a Function: Call a chaincode function without a transaction. Any changes to the state will not be persisted
*  Set stakeholders: List of members or groups who can run the chaincode. Default is everyone
*  Get stakeholders: Return the list of stakeholders

### Blockchain
*  Get Blockchain: Return info about the ledger such as height, current block hash, etc
*  Get Block by Number: Return info about a block (JSON data structure)
*  Get Block Count

### Network
*  Get Network: Return number of nodes, ports, version, timestamp
*  Shutdown: Begin the network shutdown procedure

### Storage
*  Store Document: Store and return the hash of the document
*  Store Documents: Store an array of documents and return the array of hashes
*  Get Document: Return the document identified by the hash
*  Get Documents: Return the documents identified by the array of hashes

### Event
*  Transaction Committed: Triggered on new transaction satisfied a condition (filter)
*  Block Added: Triggered when a new block added, including block height


## Application Model
<table>
<col>
<col>
<tr>
<td width="50%"><img src="refarch-app.png"></td>
<td valign="top">
An Openchain application follows a MVC-B architecture – Model, View, Control, BlockChain.
<p><p>

<ul>
  <li>VIEW LOGIC – Mobile or Web UI interacting with control logic.</li>
  <li>CONTROL LOGIC – Coordinates between UI, Data Model and OpenChain APIs to drive transitions and chain-code.</li>
  <li>DATA MODEL – Application Data Model – manages off-chain data, including Documents and large files.</li>
  <li>BLOCKCHAIN  LOGIC – Blockchain logic are extensions of the Controller Logic and Data Model, into the Blockchain realm.    Controller logic is enhanced by chaincode, and the data model is enhanced with transactions on the blockchain.</li>
</ul>
<p>
For example, a Bluemix PaaS application using Node.js might have a Web front-end user interface or a native mobile app with backend model on Cloudant data service. The control logic may interact with 1 or more chaincodes to process transactions on the blockchain.

</td>
</tr>
</table>


## Network Topology
There are 3 potential deployment models: Cloud hosted 1 network, cloud hosted multiple networks, or within each participant’s intranet.

The simplest and most efficient topology is cloud hosted 1 network, where each participant owns a number of peer nodes, including validators. Even though the network is in cloud, hosted by a vendor, who owns the physical boxes, the participants contractually control the computing resources, making it decentralized within a centralized environment.

Cloud hosted multiple networks allow participants to have their peer nodes hosted by any cloud providers, given that peer nodes can connect to one another over HTTPs.

Similar to cloud hosted multiple networks, using participants’ own networks is also possible via HTTPs channel.

## References
- [1] Mautz, R.K., H.A. Sharaf: The philosophy of auditing. Sarasota, Fl, American Accounting Association, 1961
- [2] Lorenz, E.N. Predictability; Does the Flap of a Butterfly’s Wings in Brazil Set Off a Tornado in Texas? J. Atmos. Sci. 1972, 20: 130–141.
- [3] Miguel Castro and Barbara Liskov; [Practical Byzantine Fault Tolerance] (http://www.pmg.lcs.mit.edu/papers/osdi99.pdf)
- [4] [Wikipedia Smart Contract](https://en.wikipedia.org/wiki/Smart_contract)
- [5] [“Tangaroa: a Byzantine Fault Tolerant Raft”] (http://www.scs.stanford.edu/14au-cs244b/labs/projects/copeland_zhong.pdf)
