_This document is subject to change as development progresses_

## Introduction
Enterprises today need to reduce frictions around their operations, both internal and external facing operations, to reduce their costs and lower the response times of their services to customers, employees, and suppliers.

Typically, applications that provide these services must apply Service Oriented Architecture (SOA) to communicate with each other via Application Programming Interface (API), which captures the business rules that the entity publishes for external usage. This technique has facilitated the evolution of API Economy in the last decade, where businesses monetize capabilities through APIs.

Blockchain promises to transform the integration model of many types of business applications, from banking and securities to supply-chain. The key ingredient is the shared ledger containing the necessary data locally for the applications to operate without having to call an external API, which would introduce additional complexity (access control, data transit security, trust). This is the basis of decentralization – all parties participate in the network of computer nodes to maintain the ledger such that they have exactly the same state with properties such as tamper-proof, and resilience to attacks.

Based on this principle, Openchain will be built from ground up to enhance the blockchain protocol with enterprise features for private ledgers, where participants are registered and known to the network. The new protocol (Openchain protocol) must address the following key challenges:

*  **Privacy:** Ability to protect the users from intrusions into personal matters (rights to privacy). The system must provide mechanisms to conceal identity from being publically identified by those who are not stakeholders or not authorized.
*  **Confidentiality:** Ability to protect the data from unauthorized access. This is an ethical duty that the system must provide to maintain the confidential of the transactions such that third party or outsiders cannot view the content.
*  **Auditability:** Ability to view and certify the truth of the transactions and data pertaining to a party. The system must provide sufficient knowledge about the transactions and their structures so that the auditors can determine the accuracy and reliability of accounting statement and reports [2].

Besides above challenges, as a private network, security becomes a mater of the utmost importance: Nodes must be known; access must be controlled; and identity must be verified. This will enable the network to recognize and eliminate bad and malicious entities that might try to disrupt the functioning of the network.

Beginning with a private network where nodes are known members, Openchain defines validators (via configuration) as nodes on the network responsible for maintaining the ledger; that means the validators, and only the validators, verify and process transactions then commit transactions to the ledger based on the result of the consensus. The rest of other nodes are non-validators; they receive the replicated ledger, but they don’t write to the ledger. The non-validators serve a very essential role in the network: Taking the workload off the validators in servicing the client requests.

Openchain architecture allows a pluggable consensus algorithm per deployment. That is each network may have a different consensus algorithm suitable for the requirements and usage scenarios of that network. Openchain protocol will provide a Practical Byzantine Fault Tolerance (PBFT) [4] implementation. The community or private development may supply other consensus algorithms.
Openchain protocol starts with an identity, which is a cryptographic certificate encapsulating the user’s registered confidential data managed by a Registration Authority, who can issue and revoke identities participating on the network. From this identity, the protocol will generate butterfly keys [3] for members to transact on the network. The butterfly keys conceal the identities of the transacting parties, providing the solution to the privacy requirement.

Content confidentiality is achieved by encrypting the transactions such that only the counter-parties can decrypt and execute the transactions. The encryption also includes the transaction code in its final form being deployed to blockchain. So both code and transaction content, where confidentiality is required, will be cryptographically secured at rest. The code will only be loaded and decrypted at runtime.
There are 2 types of transactions on an Openchain network: Deploying-code and invoking-function. Code running transactions is called chaincode, a decentralized transactional program, run by validators. A deploying-code transaction is to submit a chaincode to the validators, who will manage the authenticity and integrity of the code and execution. An invoking-function transaction is a call to a chaincode function (similar to a URI invoking a servlet in JEE), which may change the state of the ledger.

The concept of chaincode is more general than that of smart contract as defined by Nick Szabo [5], to emphasize the goal of bringing what he calls the "highly evolved" practices of contract law and related business practices to the design of electronic commerce protocols between strangers on the Internet. Chaincode supports any mainstream programming languages, Turing complete. The execution environment is a Docker container with Openchain context layer. So chaincode provides capabilities to define smart contract templating languages (similar to Velocity or Jade), which will restrict the functionality of the execution environment and the degree of flexibility in computing to satisfy the legal contractual requirements. Using a template language, the skills required to code a smart contract may be lowered, enabling domain knowledge professionals to write smart contracts.

Running a transaction on chaincode is always time bounded, which is configured during chaincode deployment. If a transaction times out, it is considered an error and will not effect the state of the ledger. A chaincode may call another chaincode if the callee has the same or less restrictive confidentiality scope; that is, a confidential chaincode may call a public chaincode or another confidential chaincode with the same assigned validators. [TBD: Any potential issues here?]
For confidential chaincode, when deployed, appropriate validators must be assigned since only these assigned validators can run the transactions on this chaincode. This creates quorums of validators during execution of a transaction block. Validators can make a request for the state of those transactions that they are not assigned. At the end of the successful block execution and consensus completion, the world state must be the same on all validators, whether a validator took part in executing any transaction or not.

There are REST and JSON RPC APIs, events, and an SDK for applications to communicate with Openchain. Typically applications interact with a peer node, which will require some form of authentication. Messages from a client are signed by the client identity, and verified by the peer node.

Even though Openchain doesn’t provide a native crypto-currency like Bitcoin or Ethereum, it can be easily implemented with a chaincode. Similarly for any on-chain assets, they can be encoded with chaincodes.

## Architecture
Figure 1 below shows the reference architecture aligned in 3 categories: Membership, Blockchain, and Chaincode. These catagories are logical structure, not a physical depiction of partitioning of components into separate processes, address spaces or (virtual) machines.

Some of these components will be built from ground up, some will use existing open source code as appropriate, and some will just interface with existing services to fulfill the required functions.

![Reference architecture](refarch.png)
Figure 1:  Reference architecture


At the top, CLI is the command line interface to the network. Openchain provides a set of CLI to administer and manage the network. CLI can also be used in development and test chaincodes. REST API and SDK are built on top of JSON-RPC API, which is the most complete API layer. SDK will be available in JavaScript to start, and other languages to be determined.

Event Hub in a decentralized network is complex in nature, as an event may appear to occur multiple times, each on a peer node. Callbacks would end up receiving multiple invocations for the same event. Therefore, a peer node (preferably non-validator and local) manages the event pub/sub that applications are interacting with. The peer node emits events as conditions satisfied in no particular order.
P2P Protocol uses [Google RPC](http://www.grpc.io/docs/), which is implemented over HTTP/2 standards, providing many capabilities including bidirectional streaming, flow control, multiplexing requests over a single connection. And most important of all, it works with existing Internet infrastructure – firewalls, proxies, and security. This component defines messages used by peer nodes, from point-to-point to broadcast.

Chaincode Services use [Docker](https://github.com/docker/docker) to host the chaincode without relying on any particular virtual machine or language. Docker provides a lightweight to sandbox the chaincode execution on validators.

Ledger component manages the blockchain and world state machine with 3 key attributes:

*  Efficiently calculating a cryptographic hash of the entire dataset after each block
*  Efficiently transmitting a minimal "delta" of changes to the dataset, when a peer is out of sync and needs to "catch up”
*   Minimizing the amount of data kept around necessary for each peer to operate
Ledger uses RocksDB to persist the dataset.

Consensus is an abstraction defining the interface between the consensus algorithm and the other components. Consensus receives transactions, and depending on algorithm, decides how to organize the transactions and when to execute the transactions. Successful execution of transactions results in changes to the ledger.

Registration Authority provides enrollment certificate necessary to establish identity on the network. This component is a centralized service per network deployment, and all parties control it. An endpoint (node, client, device) must register for an identity in order to join the network. Registration Authority service also acts as a network bootstrap node: Allowing peer nodes to discover and connect to each other.

Reputation Manager and Attribute Authority manage the issuance of butterfly keys and linkage with identities.

External Store is a document service like DocuSign or Cloudant, where documents may be stored and hashed to ensure the integrity of the content.

The implementation is written entirely in Golang, except pre-requisite packages such as RocksDB, which will be included in the build to create the final executable binary.

## Protocol
A peer node connects to and accepts connections from other peer nodes to form a network, communicating over gRPC. A peer node may discover other peer nodes by sending a request to the Registration Authority, which will respond with a set of “nearby” peers.

Each new connection must establish the proper handshake by sending HELLO message to each other:
*  Version: Openchain version, starting with 1.00
*  NodeID: Identity of the peer
*  MsgID: Message identifier

Upon verifying the proper handshake, the peer sets up a session then other messages may be flowed.
[TODO: How to properly verify nodes? Jeff to fill in more info]

## Consensus
Openchain provides a pluggable architecture such that different consensus algorithms may be plugged in via configuration. Openchain is for multiple private networks, each for a set of customers or a consortium of partners. Some networks may require BFT, while some others, RAFT may be applicable.

Openchain provides an implementation of Byzantine Agreement with advanced features in fault tolerance and scalability.

Transaction throughput is expected to be few thousands per second. The number of transactions per block is limited to size and time.

Only the validators perform the consensus. Each chaincode may be assigned a subset of validators, which makes up quorums. A quorum run the assigned transactions but not independently carries out the consensus. All validators participate in the consensus for a block of transactions to determine whether or not to commit the block to the ledger.

## Chaincode Host
Docker
Dockfile for each supported language:  Golang, Nodejs
Chaincode context: How chaincode access data on ledger

## Ledger
The ledger is comprised of two components. The blockchain and the state. Both will be stored in RocksDB, an embeddable persistent key-value store for fast storage. All data will be stored as key-value byte arrays.

### Blockchain
The blockchain can be thought of very simply as an array of blocks. Each block contains the following pieces of information.

* The ID of the peer that proposed the block.
* An array of transactions contained in the block.
* The hash of the previous block.
* The hash of the state after running the transactions contained in the block.

As mentioned previously, there are two types of transactions, transactions that deploy chaincode and transactions that invoke a function in chaincode. These transaction types contain the necessary information for a peer node to execute the transaction.

All hashes are calculated using the SHA3 SHAKE256 algorithm with an output size of 512 bits. [7]

### State
The state represents the state for all chaincodes. Each chaincode is assigned its own state that can used to store data in a key-value format where keys and values are arbitrary byte arrays. The state also contains the number of the block to which it corresponds.

As transactions are run in a new block, the delta from the state in the last block on the blockchain is maintained. If consensus is reached for the current block, the state changes are committed to the database and the state block number is incremented by one. If peers do not reach consensus, the delta is discarded and the database is not modified.

An important function of the state is that a hash of the entire data structure can be quickly generated after changes are made. This is because the hash of the state needs to be stored in each block committed to the blockchain.

A peer that downloads the state can fetch the block number to which the state corresponds from the state, fetch that block from the blockchain, and then compare the state hash stored in the block to the hash of the state that was downloaded. This will verify that the state is in fact accurate and that no tampering or errors have occurred during transmission.

It should be noted that the state for blocks prior to the current block is not maintained in the database. To reconstruct the state for a previous block, it is necessary to run all transactions from the genesis block to the block for which the state is desired. Finally, chaincodes can only access their own state and not the state of other chaincodes.

## Security
Authentication
Authorization
Message security

## Application Programming Interface
The Openchain API can be divided into the following categories:
*  Identity - Enrollment to get certificates or revoking a certificate
*  Address - Target and source of a transaction
*  Transaction - Unit of execution on the ledger
*  Chaincode - Program running on the ledger
*  Blockchain - Content of the ledger
*  Network - Information about the blockchain network
*  Storage - Sidecar to store files or documents whose hashes are keys
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
*  Transaction Added: Triggered on new transaction fits a condition (filter)
*  Block Added: Triggered when a new block added, including block height

## Application Model
Application using Openchain is like any other technology: An additional service layer with API and programming model. Typically that consists of user interface, some backend code, and 1 or more chaincodes to record transactions on the ledger.
[TODO: Simple example of a Bluemix application with Openchain]

## Network Topology
There are 3 potential deployment models: Cloud hosted 1 network, cloud hosted multiple networks, or within each participant’s intranet.

The simplest and most efficient topology is cloud hosted 1 network, where each participant owes a number of peer nodes, including validators. Even though the network is in cloud, hosted by a vendor, who owes the physical boxes, but the participants contractually control the computing resource, making it a decentralized within a centralized environment.

Cloud hosted multiple networks allows participants to have their peer nodes hosted by any cloud providers, given that peer nodes can connect to one another over HTTPs channel.

Similar to cloud hosted multiple networks, using participants’ own networks also possible via HTTPs channel.

## References
- [1] [Bitcoin](https://en.wikipedia.org/wiki/Bitcoin)
- [2] Mautz, R.K., H.A. Sharaf: The philosophy of auditing. Sarasota, Fl, American Accounting Association, 1961
- [3] Lorenz, E.N. Predictability; Does the Flap of a Butterfly’s Wings in Brazil Set Off a Tornado in Texas? J. Atmos. Sci. 1972, 20: 130–141.
- [4] Miguel Castro and Barbara Liskov; [Practical Byzantine Fault Tolerance] (http://www.pmg.lcs.mit.edu/papers/osdi99.pdf)
- [5] [Wikipedia Smart Contract](https://en.wikipedia.org/wiki/Smart_contract)
- [6] [“Tangaroa: a Byzantine Fault Tolerant Raft”] (http://www.scs.stanford.edu/14au-cs244b/labs/projects/copeland_zhong.pdf)
- [7] [FIPS PUB 202 "SHA-3 Standard: Permutation-Based Hash and
Extendable-Output Functions"] (http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.202.pdf)
