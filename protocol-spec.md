# Open Blockchain Protocol Specification

_Draft 0.01_

## Preface
This document is the protocol specification of Open Blockchain (OBC), a permissioned blockchain implementation for industry use-cases. It is not intended to be a complete explanation of the implementation, but rather the interfaces and relationships between components in the system and the application.

### Intended Audience
The intended audience for this specification includes the following groups:

- Blockchain vendors who want to implement blockchain systems that conform to this specification
- Tool developers who want to extend the capabilities of Open Blockchain
- Application developers who want to leverage blockchain technologies to enrich their applications

### Authors
These are the authors who wrote various sections of this document:  Binh Q Nguyen, Elli Androulaki, Angelo De Caro, Sheehan Anderson, Manish Sethi, Thorsten Kramp, Alessandro Sorniotti, Marko Vukolic, Florian Simon Schubert, Jason K Yellick, Konstantinos Christidis, Srinivasan Muralidharan, Anna D Derbakova, Dulce Ponceleon, David Kravitz, Diego Masini

### Reviewers
Frank Lu, John Wolpert, Bishop Brock, Nitin Gaur, Sharon Weed

### Acknowledgements
The following individuals have provided invaluable technical input to the specification:
Gennaro Cuomo, Joseph A Latone, Christian Cachin
________________________________________________________

## Table of Contents
#### 1. Introduction

   - 1.1 What is Open Blockchain?
   - 1.2 Why Open Blockchain?
   - 1.3 Terminology

#### 2. Fabric

   - 2.1 Architecture
   - 2.1.1 Membership Services
   - 2.1.2 Blockchain Services
   - 2.1.3 Chaincode Services
   - 2.1.4 Event Stream
   - 2.1.5 Application Programming Interface
   - 2.1.6 Command Line Interface
   - 2.2 Topology
   - 2.2.1 Single Validating Peer
   - 2.2.2 Multiple Validating Peers
   - 2.2.3 Multichain

#### 3. Protocol

   - 3.1 Message
   - 3.1.1 Discovery Messages
   - 3.1.2 Transaction Messages
   - 3.1.2.1 Transaction Data Structure
   - 3.1.2.2 Transaction Specification
   - 3.1.2.3 Deploy Transaction
   - 3.1.2.4 Invoke Transaction
   - 3.1.2.5 Query Transaction
   - 3.1.3 Synchronization Messages
   - 3.1.4 Consensus Messages
   - 3.2 Ledger
   - 3.2.1 Blockchain
   - 3.2.1.1 Block
   - 3.2.1.2 Block Hashing
   - 3.2.1.3 NonHashData
   - 3.2.1.4 Transaction
   - 3.2.2 World State
   - 3.2.2.1 Hashing the world state
   - 3.2.2.1.1 Bucket-tree
   - 3.3 Chaincode
   - 3.3.1 Chaincode Development
   - 3.3.2 Chaincode Runtime
   - 3.3.3 Chaincode Protocol
   - 3.3.3.1 Chaincode Initialization
   - 3.3.3.2 Chaincode Invoke
   - 3.3.3.3 Chaincode Query
   - 3.3.3.4 Chaincode State
   - 3.4 Pluggable Consensus Framework
   - 3.4.1 Consenter interface
   - 3.4.2 Consensus Programming Interface
   - 3.4.3 Inquirer interface
   - 3.4.4 Communicator interface
   - 3.4.5 SecurityUtils interface
   - 3.4.6 LedgerStack interface
   - 3.4.7 Executor interface
   - 3.4.7.1 Beginning a transaction batch
   - 3.4.7.2 Executing transactions
   - 3.4.7.3 Committing and rolling-back transactions
   - 3.4.8 Ledger interface
   - 3.4.8.1 ReadOnlyLedger interface
   - 3.4.8.2 UtilLedger interface
   - 3.4.8.3 WritableLedger interface
   - 3.4.9 RemoteLedgers interface
   - 3.4.10 Controller package
   - 3.4.11 Helper package
   - 3.5 Events
   - 3.4.1 Event Stream
   - 3.4.2 Event Structure
   - 3.4.3 Event Adapters

#### 4. Security
   - 4. Security
   - 4.1 Business security requirements
   - 4.1.1 Preserving user privacy while incorporating identity and role management
   - 4.1.2 Supporting invocation access control without precluding privacy
   - 4.1.3 Addressing reputation/risk management
   - 4.1.4 Enabling a system transactions capability
   - 4.1.5 Flexibly accommodating usage of transaction certificates
   - 4.1.6 Expanding to support root CA functionality
   - 4.2 User Privacy through Membership Services
   - 4.2.1 User/Client Enrollment Process
   - 4.3 Transaction security offerings at the infrastructure level
   - 4.4 Transaction Confidentiality
   - 4.5 Deployment transaction
   - 4.6 Invocation transaction
   - 4.7 Invocation Access Control offered at the infrastructure
   - 4.8 Replay Attack Resistance
   - 4.9 Access control on the application level
   - 4.9.1 Invocation access control
   - 4.9.2 Support from the fabric layer
   - 4.9.3 Implementing Invocation Access Control inside the Application
   - 4.9.4 Read Access Control Enforcement by the Application
   - 4.9.5 Support from the fabric layer.
   - 4.9.6 Enforcement of read-access control on the application.
   - 4.10 Network security (TLS)
   - 4.11 Restrictions in the current release
   - 4.11.1 Simplified Transaction Confidentiality

#### 5. Byzantine Consensus
   - 5.1 Overview
   - 5.2 Core PBFT
   - 5.3 Inner Consensus Programming Interface
   - 5.4 Sieve Consensus

#### 6. Application Programming Interface
   - 6.1 REST Service
   - 6.2 REST API
   - 6.3 CLI

#### 7. Application Model
   - 7.1 Composition of an Application
   - 7.2 Sample Application

#### 8. Future Directions
   - 8.1 Enterprise Integration
   - 8.2 Performance and Scalability
   - 8.3 Additional Consensus Plugins
   - 8.4 Additional Languages

#### 9. References

________________________________________________________

## 1. Introduction
This document specifies the principles, architecture, and protocol of Open Blockchain, a blockchain implementation suitable for industrial use-cases.

### 1.1 What is Open Blockchain?
Open Blockchain is a ledger of digital events, called transactions, shared among  different participants, each having a stake in the system. The ledger can only be updated by consensus of the participants, and, once recorded, information can never be altered. Each recorded event is cryptographically verifiable with proof of agreement from the participants.

Open Blockchain transactions are secured, private, and confidential. Each participant registers with proof of identity to the network membership services to gain access to the system. Transactions are issued with derived certificates unlinkable to the individual participant, offering a complete anonymity on the network. Transaction content is encrypted with sophisticated key derivation functions to ensure only intended participants may see the content, protecting the confidentiality of the business transactions.

Open Blockchain ledger allows compliance with regulations, and ledger entries are auditable in whole or in part. In collaboration with participants, auditors may obtain time-based certificates to allow viewing the ledger and linking transactions to provide an accurate assessment of the operations.

Techonologically, Open Blockchain is a fabric of blockchain, where Bitcoin could be a simple application built on Open Blockchain. It is a modular architecture allowing components to be plug-and-play by implementing this protocol specification. It features powerful container technology to host any main stream language for smart contracts development. Leveraging familiar and proven technologies is the moto of the fabric architecture.

### 1.2 Why Open Blockchain?

Early blockchain technology serves a set of purposes but is often not well-suited for the needs of specific industries. To meet the demands of modern markets, Open Blockchain is based on an industry-focused design that addresses the multiple and varied requirements of specific industry use cases , extending the learning of the pioneers in this field while also addressing issues such as scalability. Open Blockchain provides a new approach to enable permissioned networks, privacy, and confidentially on multiple blockchain networks.

### 1.3 Terminology
These terminologies are defined within the limited scope of this specification to help readers understand clearly and precisely the concepts described here.

**Transaction** is a request to the blockchain to execute a function on the ledger. The function is implemented by a **chaincode**.

**Transactor** is an entity that issues transactions such as a client application.

**Ledger** is a sequence of cryptographically linked blocks, containing transactions and current **world state**.

**World State** Results of executing transactions may be persisted in variables, called world state.

**Chaincode** is an application-level code (a.k.a [smart contract](https://en.wikipedia.org/wiki/Smart_contract)) stored on the ledger part of a transaction. Chaincode runs transactions that may modify the world state.

**Validating Peer** A computer node on OBC network responsible for running consensus, validating transactions, and maintaining the ledger.

**Non-validating Peer** A computer node on OBC network functions as a proxy connecting transactors to the neighboring validating peers. Non-validating peer doesn't execute transactions but does verify transactions. It also hosts event stream and REST API service.

**Permissioned Ledger** Each entity or node on the blockchain network is required to be a member of the network. Anonymous nodes are not allowed to connect.

**Privacy** Transactors need privacy to conceal their identities on the network. While members of the network may examine the transactions, but the transactions can't be linked to the transactor without special privilege.

**Confidentiality** is the ability to render the transaction content inaccessible to anyone other than the stakeholders of the transaction.

**Auditability** While private transactions are important, business usage of blockchain needs to comply with regulations and make it easy for regulators to investigate transaction records.



## 2. Fabric

Open Blockchain fabric is made up of the core components described in the subsections below.

### 2.1 Architecture
The reference architecture is aligned in 3 categories: Membership, Blockchain, and Chaincode services. These categories are a logical structure, not a physical depiction of partitioning of components into separate processes, address spaces or (virtual) machines.

![Reference architecture](images/refarch.png)

##### 2.1.1 Membership Services
Membership provides services for managing identity, privacy, and confidentiality on the network. Participants register to get identities, which will enable the Attribute Authority to issue security keys to transact. Reputation Manager enables auditors to see transactions pertaining to a participant. Of course, auditors will have to be granted proper access authority by the participants.

##### 2.1.2 Blockchain Services
Blockchain services manage the distributed ledger through a peer-to-peer protocol, built on HTTP/2. The data structures are highly optimized to provide the most efficient hash algorithm for maintaining the world state replication. Different consensus (PBFT, Raft, PoW, PoS) may be plugged in and configured per deployment.

##### 2.1.3 Chaincode Services
Chaincode services provides a secured and lightweight way to sandbox the chaincode execution potentially on the validating nodes. The environment is a “locked down” and secured container along with a set of signed base images containing secure OS and chaincode language, runtime and SDK layers for Go, Java, and Node.js. Other languages can be enabled if required.

##### 2.1.4 Event Stream
Validating peers and chaincodes can emit events on the network that applications may listen and take actions on. There is a set of pre-defined events, and chaincodes can generate custom events. Events are consumed by 1 or more event adapters. Adapters may further deliver events using other vehicles such as Web hooks or Kafka.

##### 2.1.5 Application Programming Interface (API)
The primary interface to OBC is a REST API and its variations over Swagger 2.0. The API allows applications to register users, query the blockchain, and to issue transactions. There is a set of APIs specifically for chaincode to interact with the stack to execute transactions and query transaction results.

##### 2.1.6 Command Line Interface (CLI)
CLI includes a subset of APIs to enable developers to quickly test chaincodes or query for status of transactions. CLI is implemented in Golang and operable on multiple OS platforms.

### 2.2 Topology
A deployment of Open Blockchain may consist of a membership services, many validating peers, non-validating peers, and 1 or more applications. All makes up a chain. There can be multiple chains; each has own operating parameters and security concerns.

### 2.2.1 Single Validating Peer
Functionally, a non-validating peer is a subset of a validating peer; that is, every capability on a non-validating peer may be enabled on a validating peer, so the simplest blockchain network of OBC may consist of a single validating node. This configuration is most appropriate for development environment, where a validating peer may be started up as needed during edit-compile-debug cycle.

![Single Validating Peer](images/top-single-peer.png)

A single validating peer doesn't require consensus, so by default, it uses `noops` plugin, which executes transactions as they arrive. This gives the developer an immediate feedback during development.

### 2.2.2 Multiple Validating Peers
Production or test networks should be made up of multiple validating and non-validating peers as necessary. Non-validating peers can take some workload off the validating peers, such as handling API requests and processing events.

![Multiple Validating Peers](images/top-multi-peer.png)

The validating peers form a mesh-network (every peer connects to every other peer) to disseminate information. A non-validating peer connects to a neighboring validating peer that it is allowed to connect to. Non-validating peers are optional since applications may communicate directly with validating peers.

### 2.2.3 Multichain
Each network of validating and non-validating peers makes up a chain. Many chains may be created to address different needs, similar to having multiple Web sites, each serves a different purpose.


## 3. Protocol
Open Blockchain peer-to-peer communication is built on [gRPC](http://www.grpc.io/docs/), which allows bi-directional stream-based messaging. It uses [Protocol Buffers](https://developers.google.com/protocol-buffers) to serialize data structures for  data transfer between peers. Protocol buffers are a language-neutral, platform-neutral extensible mechanism for serializing structured data. OBC data structures, messages, and services are described using [proto3 language](https://developers.google.com/protocol-buffers/docs/proto3) notation.

### 3.1 Message
Messages flowed between nodes are encapsulated by `OpenchainMessage` proto structure, which consists of 4 types: Discovery, Transaction, Synchronization, and Consensus. Each type may define more subtypes embedded in the `payload`.

```
message OpenchainMessage {
   enum Type {
        UNDEFINED = 0;

        DISC_HELLO = 1;
        DISC_DISCONNECT = 2;
        DISC_GET_PEERS = 3;
        DISC_PEERS = 4;
        DISC_NEWMSG = 5;

        CHAIN_STATUS = 6;
        CHAIN_TRANSACTION = 7;
        CHAIN_GET_TRANSACTIONS = 8;
        CHAIN_QUERY = 9;

        SYNC_GET_BLOCKS = 11;
        SYNC_BLOCKS = 12;
        SYNC_BLOCK_ADDED = 13;

        SYNC_STATE_GET_SNAPSHOT = 14;
        SYNC_STATE_SNAPSHOT = 15;
        SYNC_STATE_GET_DELTAS = 16;
        SYNC_STATE_DELTAS = 17;

        RESPONSE = 20;
        CONSENSUS = 21;
    }
    Type type = 1;
    bytes payload = 2;
    google.protobuf.Timestamp timestamp = 3;
}
```
The `payload` is an opaque byte array containing other objects such as `Transaction` or `Response` depending on the type of the message. For example, if the `type` is `CHAIN_TRANSACTION`, the `payload` is a `Transaction` object.

### 3.1.1 Discovery Messages
Upon start up, a peer runs discovery protocol if `OPENCHAIN_PEER_DISCOVERY_ROOTNODE` is specified. `OPENCHAIN_PEER_DISCOVERY_ROOTNODE` is the IP address of another peer on the network (any peer) that serves as the starting point for discovering all the peers on the network. The protocol sequence begins with `DISC_HELLO`, whose `payload` is a `HelloMessage` object, containing its endpoint:

```
message HelloMessage {
  PeerEndpoint peerEndpoint = 1;
  uint64 blockNumber = 2;
}
message PeerEndpoint {
    PeerID ID = 1;
    string address = 2;
    enum Type {
      UNDEFINED = 0;
      VALIDATOR = 1;
      NON_VALIDATOR = 2;
    }
    Type type = 3;
    bytes pkiID = 4;
}

message PeerID {
    string name = 1;
}
```

**Definition of fields:**

- `PeerID` is any name given to the peer at start up or defined in config file
- `PeerEndpoint` describes the endpoint and whether it's a validating or non-validating peer
- `pkiID` is the cryptographic ID of the peer
- `address` is host or IP address and port of the peer in the format `ip:port`
- `blockNumber` is the height of the blockchain the peer current has

If the block height received upon `DISC_HELLO` is higher than the current block height of the peer, it immediately initiates synchronization protocol to catch up with the network.

After `DISC_HELLO`, peer sends `DISC_GET_PEERS` periodically to discover any additional peers joining the network. Response to `DISC_GET_PEERS`, a peer sends `DISC_PEERS` with `payload` containing an array of `PeerEndpoint`. Other discovery message types are not used at this point.

### 3.1.2 Transaction Messages
There are 2 types of transactions: Deploy and Invoke transactions. Deploy transaction installs the specified chaincode on the chain, invoke and query transactions call a function of a deployed chaincode. Another type in consideration is Create transaction where a deployed chaincode may be instantiated on the chain and is addressable. This type has not been implemented as of this writing.

### 3.1.2.1 Transaction Data Structure

Messages with type `CHAIN_TRANSACTION` or `CHAIN_QUERY` carry in the `payload` a `Transaction` object:

```
message Transaction {
    enum Type {
        UNDEFINED = 0;
        CHAINCODE_NEW = 1;
        CHAINCODE_UPDATE = 2;
        CHAINCODE_EXECUTE = 3;
        CHAINCODE_QUERY = 4;
        CHAINCODE_TERMINATE = 5;
    }
    Type type = 1;
    string uuid = 5;
    bytes chaincodeID = 2;
    bytes payload = 3;

    ConfidentialityLevel confidentialityLevel = 7;
    bytes nonce = 8;
    bytes cert = 9;
    bytes signature = 10;

    bytes metadata = 4;
    google.protobuf.Timestamp timestamp = 6;
}

enum ConfidentialityLevel {
    PUBLIC = 0;
    CONFIDENTIAL = 1;
}

```
**Definition of fields:**
- `type` - The type of transaction, which is 1 of the following:
	- `UNDEFINED` - Reserved for future use.
    - `CHAINCODE_NEW` - Represents the deployment of a new chaincode.
	- `CHAINCODE_UPDATE` - Reserved for future use.
	- `CHAINCODE_EXECUTE` - Represents a chaincode function execution that may read and modify the world state.
	- `CHAINCODE_QUERY` - Represents a chaincode function execution that may only read the world state.
	- `CHAINCODE_TERMINATE` - Marks a chaincode as inactive so that future functions of the chaincode can no longer be invoked.
- `chaincodeID` - The ID of a chaincode which is a hash of the chaincode source, path to the source code, constructor function, and parameters.
- `payload` - Bytes defining the payload of the transaction.
- `metadata` - Bytes defining any associated transaction metadata that the application may use.
- `uuid` - A unique ID for the transaction.
- `timestamp` - A timestamp of when the transaction request was received by the peer.
- `confidentialityLevel` - Level of data confidentiality. There are 2 levels currently. Future releases may define more levels.
- `nonce` - Used for security
- `cert` - Certificate of the transactor
- `signature` - Signature of the transactor

More detail on transaction security in section 4.

### 3.1.2.2 Transaction Specification
A transaction is always associated with a chaincode specification which defines the chaincode and the execution environment such as language and security context. Currently only golang is supported for writing chaincode; other languages may be added in the future.

```
message ChaincodeSpec {
    enum Type {
        UNDEFINED = 0;
        GOLANG = 1;
        NODE = 2;
    }
    Type type = 1;
    ChaincodeID chaincodeID = 2;
    ChaincodeInput ctorMsg = 3;
    int32 timeout = 4;
    string secureContext = 5;
    ConfidentialityLevel confidentialityLevel = 6;
    bytes metadata = 7;
}

message ChaincodeID {
    string path = 1;
    string name = 2;
}

message ChaincodeInput {
    string function = 1;
    repeated string args  = 2;
}
```
**Definition of fields:**
- `chaincodeID` - The chaincode source code path and name.
- `ctorMsg` - Function name and argument parameters to call.
- `timeout` - Time in millis to execute the transaction.
- `confidentialityLevel` - Confidentiality level of this transaction.
- `secureContext` - Security context of the transactor.
- `metadata` - Any data the application wants to pass along.

The peer, receiving the `chaincodeSpec`, wraps it in an appropriate transaction message and broadcasts to the network.

### 3.1.2.3 Deploy Transaction
A deploy transaction `type` is `CHAINCODE_NEW` and the payload contains an object of `ChaincodeDeploymentSpec`.

```
message ChaincodeDeploymentSpec {
    ChaincodeSpec chaincodeSpec = 1;
    google.protobuf.Timestamp effectiveDate = 2;
    bytes codePackage = 3;
}
```
**Definition of fields:**
- `chaincodeSpec` - See section 3.1.2.2 above.
- `effectiveDate` - Time when the chaincode is ready to accept invocations.
- `codePackage` - gzip of the chaincode source.

The validating peers always verify the hash of the `codePackage` when deploy the chaincode to make sure the package has not been tampered with since the deploy transaction enterred the network.

### 3.1.2.4 Invoke Transaction
An invoke transaction `type` is `CHAINCODE_EXECUTE` and the `payload` contains an object of `ChaincodeInvocationSpec`.
```
message ChaincodeInvocationSpec {
    ChaincodeSpec chaincodeSpec = 1;
}
```

### 3.1.2.5 Query Transaction
A query transaction is similar to an invoke transaction, but the message `type` is `CHAINCODE_QUERY`.

### 3.1.3 Synchronization Messages
Synchronization protocol starts with discovery, described above in section 3.1.1, when a peer realizes that it's behind or its current block is not the same with others. A peer broadcasts either `SYNC_GET_BLOCKS`, `SYNC_STATE_GET_SNAPSHOT`, or `SYNC_STATE_GET_DELTAS` and receives `SYNC_BLOCKS`, `SYNC_STATE_SNAPSHOT`, or `SYNC_STATE_DELTAS` respectively.

The installed consensus plugin (eg pbft) dictates how synchronization protocol is being applied. Each message is designed for specific situation:

**SYNC_GET_BLOCKS** requests for a range of contiguous blocks expressed in the message `payload`, which is an object of `SyncBlockRange`.
```
message SyncBlockRange {
    uint64 start = 1;
    uint64 end = 2;
}
```
A receiving peer responds with a `SYNC_BLOCKS` message whose `payload` contains an object of `SyncBlocks`
```
message SyncBlocks {
    SyncBlockRange range = 1;
    repeated Block blocks = 2;
}
```
The `start` and `end` indicate the starting and ending blocks inclusively. The order in which blocks are returned is defined by the `start` and `end` values. For example, if `start`=3 and `end`=5, the order of blocks will be 3, 4, 5. If `start`=5 and `end`=3, the order will be 5, 4, 3.

**SYNC_STATE_GET_SNAPSHOT** requests for the snapshot of the current world state. The `payload` is an object of `SyncStateSnapshotRequest`
```
message SyncStateSnapshotRequest {
  uint64 correlationId = 1;
}
```
The `correlationId` is used by the requesting peer to keep track of the response messages. A receiving peer replies with `SYNC_BLOCKS` message whose `payload` is an instance of `SyncStateSnapshot`
```
message SyncStateSnapshot {
    bytes delta = 1;
    uint64 sequence = 2;
    uint64 blockNumber = 3;
    SyncStateSnapshotRequest request = 4;
}
```
This message contains the snapshot or a chunk of the snapshot on the stream, and in which case, the sequence indicate the order starting at 0.  The terminating message will have len(delta) == 0.

**SYNC_STATE_GET_DELTAS** requests for the state deltas of a range of contiguous blocks. By default, the Ledger maintains 500 transition deltas. A delta(j) is a state transition between block(i) and block(j) where i = j-1. The message `payload` contains an instance of `SyncStateDeltasRequest`
```
message SyncStateDeltasRequest {
    SyncBlockRange range = 1;
}
```
A receiving peer responds with `SYNC_STATE_DELTAS`, whose `payload` is an instance of `SyncStateDeltas`
```
message SyncStateDeltas {
    SyncBlockRange range = 1;
    repeated bytes deltas = 2;
}
```
A delta may be applied forward (from i to j) or backward (from j to i) in the state transition.

### 3.1.4 Consensus Messages
Consensus deals with transactions, so a `CONSENSUS` message is initiated internally by the consensus framework when it receives a `CHAIN_TRANSACTION` message. The framework converts `CHAIN_TRANSACTION` into `CONSENSUS` then broadcasts to the validating nodes with the same `payload`. The consensus plugin receives this message and process according to its internal algorithm. The plugin may create custom subtypes to manage consensus finite state machine. See section 3.4 for more details.


### 3.2 Ledger

The ledger consists of two primary pieces, the blockchain and the world state. The blockchain is a series of linked blocks that is used to record transactions within the ledger. The world state is a key-value database that chaincodes may use to store state when executed by a transaction.

### 3.2.1 Blockchain

#### 3.2.1.1 Block

The blockchain is defined as a linked list of blocks as each block contains the hash of the previous block in the chain. The two other important pieces of information that a block contains are the list of transactions contained within the block and the hash of the world state after executing all transactions in the block.

```
message Block {
  version = 1;
  google.protobuf.Timestamp timestamp = 2;
  repeated Transaction transactions = 3;
  bytes stateHash = 4;
  bytes previousBlockHash = 5;
  bytes consensusMetadata = 6;
  NonHashData nonHashData = 7;
}
```
* `version` - Version used to track any protocol changes.
* `timestamp` - The timestamp to be filled in by the block proposer.
* `transactions` - An array of Transaction messages.
* `stateHash` - The hash of the world state.
* `previousBlockHash` - The hash of the previous block.
* `consensusMetadata` - Optional metadata that the consensus may include in a block.
* `nonHashData` - A NonHashData message that is set to nil before computing the hash of the block, but stored as part of the block in the database.

#### 3.2.1.2 Block Hashing

* Hashing: The previousBlockHash hash is calculated using the following algorithm.
  1. Serialize the Block message to bytes using the protocol buffer library.

  2. Hash the serialized block message to 512 bits of output using the SHA3 SHAKE256 algorithm as described in [FIPS 202](http://csrc.nist.gov/publications/drafts/fips-202/fips_202_draft.pdf).


#### 3.2.1.3 NonHashData

The NonHashData message is used to store block metadata that is not required to be the same value on all peers. These are suggested values.

```
message NonHashData {
  google.protobuf.Timestamp localLedgerCommitTimestamp = 1;
  repeated TransactionResult transactionResults = 2;
}

message TransactionResult {
  string uuid = 1;
  bytes result = 2;
  uint32 errorCode = 3;
  string error = 4;
}
```

* `localLedgerCommitTimestamp` - A timestamp indicating when the block was commited to the local ledger.

* `TransactionResult` - An array of transaction results.

* `TransactionResult.uuid` - The ID of the transaction.

* `result` - The return value of the transaction.

* `errorCode` - A code that can be used to log errors associated with the transaction.

* `error` - A string that can be used to log errors associated with the transaction.


#### 3.2.1.4 Transaction Execution

A transaction defines either the deployment of a chaincode or the execution of a chaincode. All transactions within a block are run before recording a block in the ledger. When chaincodes execute, they may modify the world state. The hash of the world state is then recorded in the block.


### 3.2.2 World State

#### 3.2.2.1 Hashing the world state

#### 3.2.2.1.1 Bucket-tree

The *world state* is assumed to be a collection of *key-values*. A key is composed of two components namely chaincodeID and keyWithinChaincode. For the purpose of the description below, the former component is assumed to be a valid utf8 string and the later component can be an arbitrary bytes and further, the two components of the key are assumed to be separated by a `nil` byte.

This method models a *merkle-tree* on top of buckets of a *hash table* in order to compute the crypto-hash of the *world state*.

At the core of this method, the *key-values* of the world state are assumed to be stored in a hash-table that consists of a pre-decided number of buckets (`numBuckets`). A hash function (`hashFunction`) is employed to determine the bucket number that contains a given key.

For modeling the merkle-tree, the ordered buckets act as leaf nodes of the tree - lowest numbered bucket being the left most leaf node in the tree. For constructing the second-last level of the tree, a pre-decided number of leaf nodes (`maxGroupingAtEachLevel`), starting from left, are grouped together and for each such group, a node is inserted at the second-last level that acts as a common parent for all the leaf nodes in the group. Note that the number of children for the last parent node may be less than `maxGroupingAtEachLevel`. This grouping method of constructing the next higher level is repeated until the root node of the tree is constructed.

An example setup with configuration `{numBuckets=10009 and maxGroupingAtEachLevel=10}` will result in a tree with number of nodes at different level as depicted in the following table.

| Level         | Number of nodes |
| ------------- |:---------------:|
| 0             | 1               |
| 1             | 2               |
| 2             | 11              |
| 3             | 101             |
| 4             | 1001            |
| 5             | 10009           |

For computing the crypto-hash of the world state, the crypto-hash of each bucket is computed and is assumed to be the crypto-hash of leaf-nodes of the merkle-tree. In order to compute crypto-hash of a bucket, the key-values present in the bucket are first serialized and crypto-hash function is applied on the serialized bytes. For serializing the key-values of a bucket, all the key-values of with a common chaincodeID prefix are serialized separately and then appending together, in the ascending order of chaincodeIDs. For serializing the key-values of a chaincodeID, following information is concatenated
   1. Length of chaincodeID (number of bytes in the chaincodeID)
   - The utf8 bytes of the chaincodeID
   - Number of key-values for the chaincodeID
   - For each key-value (in sorted order of the key)
      - Length of the key
      - key bytes
      - Length of the value
      - value bytes

For all the numeric types in the above list of items (e.g., Length of chaincodeID), protobuf's varint encoding is assumed to be used. The purpose of the above encoding is to achieve a byte representation of the key-values within a bucket that can not be arrived at by any other combination of key-values and also to reduce the overall size of the serialized bytes.

For example, consider a bucket that contains three key-values namely, `chaincodeID1_key1:value1, chaincodeID1_key2:value2, and chaincodeID2_key1:value1`. The serialized bytes for the bucket would logically look as - `12 + chaincodeID1 + 2 + 4 + key1 + 6 + value1 + 4 + key2 + 6 + value2 + 12 + chaincodeID2 + 1 + 4 + key1 + 6 + value1`

If a bucket has no key-value present, the crypto-hash is considered as `nil`.

The crypto-hash of an intermediate node and root node are computed just like in a standard merkle-tree i.e., applying a crypto-hash function on the bytes obtained by concatenating the crypto-hash of all the children nodes, from left to right. If a child has a crypto-hash as `nil`, the crypto-hash of the child is not considered while concatenating the children crypto-hashes for computing crypto-hash of the parent. If the node has a single child, the crypto-hash of the child is assumed to be the crypto-hash of the node. Finally, the crypto-hash of the root node is considered as the crypto-hash of the world state.

The above method offers performance benefits for computing crypto-hash when a few key-values change in the state. The major benefits include
  - Computation of crypto-hashes of the unchanged buckets can be skipped
  - The depth and breadth of the merkle-tree can be controlled by configuring the parameters `numBuckets` and `maxGroupingAtEachLevel`. Both depth and breadth of the tree has different implication on the performance cost incurred by and resource demand of different resources (namely - disk I/O, storage, and memory)

In a particular deployment, all the peer nodes are expected to use same values for the configurations `numBuckets, maxGroupingAtEachLevel, and hashFunction`. Further, if any of these configurations are to be changed at a later stage, the configurations should be changed on all the peer nodes so that the comparison of crypto-hashes across peer nodes is meaningful. Also, this may require to migrate the existing data based on the implementation. For example, an implementation is expected to store the last computed crypto-hashes for all the nodes in the tree which would need to be recalculated.

### 3.3 Chaincode
There are three types of transactions that can be executed by a chaincode
  - Deploy transaction - sends a request to the fabric to deploy the chaincode and returns an unique identifier for the chaincode
  - Invoke transaction - sends an "invoke" request to the chaincode and returns a transaction identifier
  - Query transaction - sends the "query" request to the chaincode and returns the results of the query

From a developer's point of view, a chaincode will implement a chaincode interface that supports two methods
  - A method to execute invoke transactions (called the `Run` method in rest of the doc)
  - A method to execute query queries (called the `Query` method in rest of the doc)

The `Run` and `Query` methods will take the name of a request function along with corresponding arguments for each of the invoke and query transactions it implements.

The Fabric provides support for persisting and retrieving key-value pairs on a per-chaincode basis. This support is accessible from the `Run` and `Query` methods. An invoke transaction can change chaincode state by modifying key-value pairs, while a query transaction should not be allowed to modify them.

### 3.3.1 Chaincode Development
A chaincode defines `Run` and `Query` methods. The `Run` method is called by the Deploy and Invoke transactions, and the `Query` method would be called Query transactions.

Additionally, on initialization the chaincode establishes a bi-directional communication stream with the validating peer that launched the chaincode. This is done using an initialization function provided by the Fabric.

The `Run` method can create, change, delete or retrieve key-value pairs, but the `Query` method to can only retrieve key-value pairs but not modify them.

The Fabric would serialize Run method calls by serializing the Deploy and Invoke transactions. However multiple Query method calls can be called concurrently.

### 3.3.2 Chaincode runtime
A high level life-cycle is as follows
  - A deploy transaction is executed on the Validating Node
    - The chaincode is extracted and is executed
    - On successful execution the chaincode creates a communication channel to the Validating Node
    - The chaincode listens on the communication channel for invoke and query requests
  - An invoke or query transaction is executed on the validating node
    - Chaincode is restarted if not running
    - invoke or query transaction is sent to the chaincode

### 3.3.3 Chaincode Protocol
The communication between the Validator Node and the chaincode is based on gRPC. The Fabric provides a reference implementation for chaincode written in golang. The chaincode performs its functions by exchanging messages on a bidirectional gRPC stream.

The approach taken by the reference implementation is to provide a thin "shim" layer which shields developer written chaincode from communication details. The shim layer handles the interaction between the user written chaincode and the Validating Node. All communication is accomplished using the protobuf "ChaincodeMessage"  structure.

Rest of this section specifies the details of that communication protocol. We use pseudo-golang syntax to describe message structures.

### 3.3.3.1 Chaincode Initialization
This section describes the interactions between chaincode and Validator Node when the Fabric launches a chaincode.

The shim layer sends a one time registration message to the Validator Node
  - ChaincodeMessage { Type: ChaincodeMessage_Type_REGISTER, Payload: marshalled_ChaincodeID } where "marshalled_ChaincodeID" is ChaincodeID{ Name: < name of the chaincode > } marshalled using gRPC

On success, the shim would receive a ChaincodeMessage with Type ChaincodeMessage_Type_REGISTERED

On failure, the shim would receive a ChaincodeMessage with Type ChaincodeMessage_Type_ERROR. The shim should close the gRPC channel and exit.

After registration, the shim receives an init message from the Validator Node
  - ChaincodeMessage { Type: ChaincodeMessage_Type_INIT, Payload: marshalled_ChaincodeInput, Uuid: "< a unique id" > } where "marshalled_ChaincodeInput" is ChaincodeInput { Function: "<name of function>", Args: { "arg1", "arg2", ..}

The shim should call the Run method of the chaincode with parameters Function and Args received from the above message.

The chaincode can return an error or a response message. The shim would send response message back to the Validator Node using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Payload: <response message as bytes>, Uuid: "the unique id received on the init message from the Validator Node" }

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the init message from the Validator Node" }

At this point the chaincode initialization is complete and is ready to receive Invoke and Query Transactions.

### 3.3.3.2 Chaincode Invoke
This section describes the interactions between the chaincode and the Validator Node when the Fabric sends an Invoke message to the chaincode. The Validator Node will serialize invoke transactions so that the shim will receive one transaction at a time.

The shim layer receives invoke transaction message from the Validator Node
  - ChaincodeMessage { Type: ChaincodeMessage_Type_TRANSACTION, Payload: marshalled_ChaincodeInput } where "marshalled_ChaincodeInput" is ChaincodeInput { Function: "<name of function>", Args: { "arg1", "arg2", ..}

The shim should call the Run method of the chaincode with parameters Function and Args received from the above message.

The chaincode can return an error or a response message. The shim would send response message back to the Validator Node using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Payload: <response message as bytes>, Uuid: "the unique id received on the request message from the Validator Node" }

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the request message from the Validator Node" }

At this point the invoke transaction is complete and the shim layer is ready to receive another invoke transaction.


### 3.3.3.3 Chaincode Query
This section describes the interactions between the chaincode and the Validator Node when the Fabric sends an Query message to the chaincode. The Validator Node can send multiple Query requests concurrently.

The shim layer receives query request message from the Validator Node
  - ChaincodeMessage { Type: ChaincodeMessage_Type_QUERY, Payload: marshalled_ChaincodeInput } where "marshalled_ChaincodeInput" is ChaincodeInput { Function: "<name of function>", Args: { "arg1", "arg2", ..}

The shim should call the Query method of the chaincode with parameters Function and Args received from the above message.

The chaincode can return an error or a response message. The shim would send response message back to the Validator Node using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Payload: <response message as bytes>, Uuid: "the unique id received on the request message from the Validator Node" }

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the request message from the Validator Node" }

At this point the query request is complete.

### 3.3.3.4 Chaincode State

The sections above dealt with how a user can send requests to the chaincode via the Fabric.This section deals with the protocol by which chaincode can store, modify and retrieve state information in the Fabric. The primitives of interaction are
  - PUT_STATE - store a (key, value) pair with the Fabric
  - GET_STATE - given a key, get its value from the Fabric
  - DEL_STATE - delete a key and its value from the Fabric
  - RANGE_QUERY_STATE - get a range of key values from the Fabric
  - INVOKE_CHAINCODE - invoke another chaincode by supplying (function name, arguments)
  - QUERY_CHAINCODE - query another chaincode by supplying (function name, arguments)

These state requests can be initiated by the chaincode as part of the processing of the init, invoke or query requests described in the previous sections (referred to as "initiating request" below).

#### 3.3.3.4.1 PUT_STATE
On receiving {key, value} from the chaincode, the shim would send the following message to the Fabric
  - ChaincodeMessage { Type: ChaincodeMessage_Type_PUT_STATE, Payload: <marshalled_PutStateInfo>, Uuid: "the unique id received on the initiating request from the Validator Node" } where "marshalled_PutStateInfo" is PutStateInfo{ Key: key, Value: value }

The Fabric can return an error or a response message which the shim would report back to the chaincode appropriately.

  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Uuid: "the unique id received on the initiating request from the Validator Node" }

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the initiating request from the Validator Node" }

#### 3.3.3.4.2 GET_STATE
On receiving a key from the chaincode, the shim would send the following message to the Fabric
  - ChaincodeMessage { Type: ChaincodeMessage_Type_GET_STATE, Payload: <marshalled_key>, Uuid: "the unique id received on the initiating request from the Validator Node" } where "marshalled_key" is the key marshalled as raw bytes.

The Fabric can return an error or a response message.

  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Payload: bytes_value, Uuid: "the unique id received on the initiating request from the Validator Node" }

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the initiating request from the Validator Node" }

The shim would extract the Payload from a successful response message or the error and send them to the chaincode.

#### 3.3.3.4.3 DEL_STATE
On receiving a key from the chaincode, the shim would send the following message to the Fabric
  - ChaincodeMessage { Type: ChaincodeMessage_Type_DEL_STATE, Payload: <marshalled_key>, Uuid: "the unique id received on the initiating request from the Validator Node" } where "marshalled_key" is the key marshalled as raw bytes.

The Fabric can return an error or a response message which the shim would report back to the chaincode appropriately.

  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Uuid: "the unique id received on the initiating request from the Validator Node" }

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the initiating request from the Validator Node" }

#### 3.3.3.4.4 RANGE_QUERY_STATE

On receiving a "start-key" an "end-key" and a "limit" from the chaincode, the shim would send the following message to the Fabric
  - ChaincodeMessage { Type: ChaincodeMessage_Type_RANGE_QUERY_STATE, Payload: <marshalled_RangeQueryStateInfo>, Uuid: "the unique id received on the initiating request from the Validator Node" } where "marshalled_RangeQueryStateInfo" is RangeQueryStateInfo{StartKey: start-key, EndKey: end-key, Limit: limit} marshalled as raw bytes.

The Fabric can return an error or a response message.

  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Payload: marshalled_RangeQueryStateResponse, Uuid: "the unique id received on the initiating request from the Validator Node" } where "marshalled_RangeQueryStateResponse" is the marshalled bytes of RangeQueryStateResponse. RangeQueryStateResponse contains an array of {key,value} pairs starting with the "start-key", ending with the "end-key" and limited by the specified by the "limit" value.

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the initiating request from the Validator Node" }

The shim would extract the Payload from a successful response message or the error and send them to the chaincode.

#### 3.3.3.4.5 INVOKE_CHAINCODE

On receiving a "chaincode name", a "function name" and "arguments" from the chaincode, the shim would send the following message to the Fabric
  - ChaincodeMessage { Type: ChaincodeMessage_Type_INVOKE_CHAINCODE, Payload: <marshalled_ChaincodeSpec>, Uuid: "the unique id received on the initiating request from the Validator Node" } where "marshalled_ChaincodeSpec" is ChaincodeSpec{ChaincodeID: <the chaincode name>, ChaincodeInput{ Function: <function name>, Args: <arguments>} } marshalled as raw bytes.

The Fabric can return an error or a response message.

  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Payload: bytes_value, Uuid: "the unique id received on the initiating request from the Validator Node" } where "bytes_value" is response from called chaincode marshalled as raw bytes.

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the initiating request from the Validator Node" }

The shim would extract the Payload from a successful response message or the error and send them to the chaincode.

#### 3.3.3.4.6 QUERY_CHAINCODE

On receiving a "chaincode name", a "function name" and "arguments" from the chaincode, the shim would send the following message to the Fabric
  - ChaincodeMessage
  { Type: ChaincodeMessage_Type_QUERY_CHAINCODE, Payload: <marshalled_ChaincodeSpec>, Uuid: "the unique id received on the initiating request from the Validator Node" } where "marshalled_ChaincodeSpec" is ChaincodeSpec{ChaincodeID: <the chaincode name>, ChaincodeInput{ Function: <function name>, Args: <arguments>} } marshalled as raw bytes.

The Fabric can return an error or a response message.

  - ChaincodeMessage { Type: ChaincodeMessage_Type_RESPONSE, Payload: bytes_value, Uuid: "the unique id received on the initiating request from the Validator Node" } where "bytes_value" is response from called chaincode marshalled as raw bytes.

and would send an error using the following message
  - ChaincodeMessage { Type: ChaincodeMessage_Type_ERROR, Payload: <response message as bytes if any>, Uuid: "the unique id received on the initiating request from the Validator Node" }

The shim would extract the Payload from a successful response message or the error and send them to the chaincode.


### 3.4 Pluggable Consensus Framework

Open Blockchain consensus framework defines the interfaces that every consensus _plugin_ implements:

  - `consensus.Consenter`: interface that  allows consensus plugin to receive messages from the network.
  - `consensus.CPI`:  _Consensus Programming Interface_ (`CPI`) is used by consensus plugin to interact with rest of the stack. This interface is split in two parts:
	  - `consensus.Communicator`: used to send (broadcast and unicast) messages to other validating peers.
	  - `consensus.LedgerStack`: which is used as an interface to the Open Blockchain execution framework as well as the ledger.

As described below in more details, `consensus.LedgerStack` encapsulates, among other interfaces, the `consensus.Executor` interface, which is the key part of the consensus framework. Namely, `consensus.Executor` interface allows for a (batch of) transaction to be started, executed, rolled back if necessary, previewed, and potentially committed. A particular property that every consensus plugin needs to satisfy is that batches (blocks)  of transactions are committed to the ledger (via `consensus.Executor.CommitTxBatch`) in total order across all validating peers (see `consensus.Executor` interface description below for more details).

Currently, consensus framework consists of 3 packages `consensus`, `controller`, and `helper`. The primary reason for `controller` and `helper` packages is to avoid "import cycle" in Go (golang) and minimize code changes for plugin to update.

- `controller` package specifies the consensus plugin used by a validating peer.
- `helper` package is a shim around a consensus plugin that helps it interact with the rest of the stack, such as maintaining message handlers to other peers.  

There are 2 consensus plugins provided: `pbft` and `noops`:

-  `obcpbft` package contains consensus plugin that implements *PBFT* [1] and *Sieve* consensus protocols. See section 5 for more detail.
-  `noops` is a ''dummy'' consensus plugin for development and test purposes. It doesn't perform consensus but processes all consensus messages. It also serves as a good simple sample to start learning how to code a consensus plugin.


### 3.4.1 `Consenter` interface

Definition:
```
type Consenter interface {
	RecvMsg(msg *pb.OpenchainMessage) error
}
```
The plugin's entry point for (external) client requests, and consensus messages generated internally (i.e. from the consensus module) during the consensus process. The `controller.NewConsenter` creates the plugin `Consenter`. `RecvMsg` processes the incoming transactions in order to reach consensus.

See `helper.HandleMessage` below to understand how the peer interacts with this interface.

### 3.4.2 `CPI` interface

Definition:
```
type CPI interface {
	Inquirer
	Communicator
	SecurityUtils
	LedgerStack
}
```
`CPI` allows the plugin to interact with the stack. It is implemented by the `helper.Helper` object. Recall that this object:

  1. Is instantiated when the `helper.NewConsensusHandler` is called.
  2. Is accessible to the plugin author when they construct their plugin's `consensus.Consenter` object.

### 3.4.3 `Inquirer` interface

Definition:
```
type Inquirer interface {
        GetNetworkInfo() (self *pb.PeerEndpoint, network []*pb.PeerEndpoint, err error)
        GetNetworkHandles() (self *pb.PeerID, network []*pb.PeerID, err error)
}
```
This interface is a part of the `consensus.CPI` interface. It is used to get the handles of the validating peers in the network (`GetNetworkHandles`) as well as details about the those validating peers (`GetNetworkInfo`):

Note that the peers are identified by a `pb.PeerID` object. This is a protobuf message (in the `protos` package), currently defined as (notice that this definition will likely be modified):

```
message PeerID {
    string name = 1;
}
```

### 3.4.4 `Communicator` interface

Definition:

```
type Communicator interface {
	Broadcast(msg *pb.OpenchainMessage) error
	Unicast(msg *pb.OpenchainMessage, receiverHandle *pb.PeerID) error
}
```

This interface is a part of the `consensus.CPI` interface. It is used to communicate with other peers on the network (`helper.Broadcast`, `helper.Unicast`):

### 3.4.5 `SecurityUtils` interface

Definition:

```
type SecurityUtils interface {
        Sign(msg []byte) ([]byte, error)
        Verify(peerID *pb.PeerID, signature []byte, message []byte) error
}
```

This interface is a part of the `consensus.CPI` interface. It is used to handle the cryptographic operations of message signing (`Sign`) and verifying signatures (`Verify`)

### 3.4.6 `LedgerStack` interface

Definition:

```
type LedgerStack interface {
	Executor
	Ledger
	RemoteLedgers
}
```

A key member of the `CPI` interface, `LedgerStack` groups interaction of consensus with the rest of the Open Blockchain blockchain fabric, such as the execution of transactions, querying, and updating the ledger.  This interface supports querying the local blockchain and state, updating the local blockchain and state, and querying the blockchain and state of other nodes in the consensus network. It consists of three parts: `Executor`, `Ledger` and `RemoteLedgers` interfaces. These are described in the following.

### 3.4.7 `Executor` interface

Definition:

```
type Executor interface {
	BeginTxBatch(id interface{}) error
	ExecTXs(id interface{}, txs []*pb.Transaction) ([]byte, []error)  
	CommitTxBatch(id interface{}, transactions []*pb.Transaction, transactionsResults []*pb.TransactionResult, metadata []byte) error  
	RollbackTxBatch(id interface{}) error  
	PreviewCommitTxBatchBlock(id interface{}, transactions []*pb.Transaction, metadata []byte) (*pb.Block, error)  
}
```

The executor interface is the most frequently utilized portion of the `LedgerStack` interface, and is the only piece which is strictly necessary for a consensus network to make progress.  The interface allows for a transaction to be started, executed, rolled back if necessary, previewed, and potentially committed.  This interface is comprised of the following methods.

#### 3.4.7.1 Beginning a transaction batch

```
BeginTxBatch(id interface{}) error
```

This call accepts an arbitrary `id`, deliberately opaque, as a way for the consensus plugin to ensure only the transactions associated with this particular batch are executed. For instance, in the pbft implementation, this `id` is the an encoded hash of the transactions to be executed.

#### 3.4.7.2 Executing transactions

```
ExecTXs(id interface{}, txs []*pb.Transaction) ([]byte, []error)
```

This call accepts an array of transactions to execute against the current state of the ledger and returns the current state hash in addition to an array of errors corresponding to the array of transactions.  Note that a transaction resulting in an error has no effect on whether a transaction batch is safe to commit.  It is up to the consensus plugin to determine the behavior which should occur when failing transactions are encountered.  This call is safe to invoke multiple times.

#### 3.4.7.3 Committing and rolling-back transactions

```
RollbackTxBatch(id interface{}) error
```

This call aborts an execution batch.  This will undo the changes to the current state, and restore the ledger to its previous state.  It concludes the batch begun with `BeginBatchTx` and a new one must be created before executing any transactions.

```
PreviewCommitTxBatchBlock(id interface{}, transactions []*pb.Transaction, metadata []byte) (*pb.Block, error)
```

This call is most useful for consensus plugins which wish to test for non-deterministic transaction execution.  The hashable portions of the block returned are guaranteed to be identical to the block which would be committed if `CommitTxBatch` were immediately invoked.  This guarantee is violated if any new transactions are executed.

```
CommitTxBatch(id interface{}, transactions []*pb.Transaction, transactionsResults []*pb.TransactionResult, metadata []byte) error
```

This call commits a block to the blockchain.  Blocks must be committed to a blockchain in total order. ``CommitTxBatch`` concludes the transaction batch, and a new call to `BeginTxBatch` must be made before any new transactions are executed and committed.


### 3.4.8 `Ledger` interface

Definition:

```
type Ledger interface {
	ReadOnlyLedger
	UtilLedger
	WritableLedger
}
```

``Ledger`` interface is intended to allow the consensus plugin to interrogate and possibly update the current state and blockchain. It is comprised of the three interfaces described below.

#### 3.4.8.1 `ReadOnlyLedger` interface

Definition:

```
type ReadOnlyLedger interface {
	GetBlock(id uint64) (block *pb.Block, err error)
	GetCurrentStateHash() (stateHash []byte, err error)
	GetBlockchainSize() (uint64, error)
}
```

`ReadOnlyLedger` interface is intended to query the local copy of the ledger without the possibility of modifying it.  It is comprised of the following functions.

```
GetBlockchainSize() (uint64, error)
```

This call returns the current length of the blockchain ledger.  In general, this function should never fail, though in the unlikely event that this occurs, the error is passed to the caller to decide what if any recovery is necessary.  The block with the highest number will have block number `GetBlockchainSize()-1`.  

Note that in the event that the local copy of the blockchain ledger is corrupt or incomplete, this call will return the highest block number in the chain, plus one.  This allows for a node to continue operating from the current state/block even when older blocks are corrupt or missing.

```
GetBlock(id uint64) (block *pb.Block, err error)
```

This call returns the block from the blockchain with block number `id`.  In general, this call should not fail, except when the block queried exceeds the current blocklength, or when the underlying blockchain has somehow become corrupt.  A failure of `GetBlock` has a possible resolution of using the state transfer mechanism to retrieve it.


```
GetCurrentStateHash() (stateHash []byte, err error)
```

This call returns the current state hash for the ledger.  In general, this function should never fail, though in the unlikely event that this occurs, the error is passed to the caller to decide what if any recovery is necessary.


#### 3.4.8.2 `UtilLedger` interface

Definition:

```
type UtilLedger interface {
	HashBlock(block *pb.Block) ([]byte, error)
	VerifyBlockchain(start, finish uint64) (uint64, error)
}
```

`UtilLedger`  interface defines some useful utility functions which are provided by the local ledger.  Overriding these functions in a mock interface can be useful for testing purposes.  This interface is comprised of two functions.

```
HashBlock(block *pb.Block) ([]byte, error)
```

Although `*pb.Block` has a `GetHash` method defined, for mock testing, overriding this method can be very useful.  Therefore, it is recommended that the `GetHash` method never be directly invoked, but instead invoked via this `UtilLedger.HashBlock` interface.  In general, this method should never fail, but the error is still passed to the caller to decide what if any recovery is appropriate.

```
VerifyBlockchain(start, finish uint64) (uint64, error)
```

This utility method is intended for verifying large sections of the blockchain.  It proceeds from a high block `start` to a lower block `finish`, returning the block number of the first block whose `PreviousBlockHash` does not match the block hash of the previous block as well as an error.  Note, this generally indicates the last good block number, not the first bad block number.



#### 3.4.8.3 `WritableLedger` interface

Definition:

```
type WritableLedger interface {
	PutBlock(blockNumber uint64, block *pb.Block) error
	ApplyStateDelta(id interface{}, delta *statemgmt.StateDelta) error
	CommitStateDelta(id interface{}) error
	RollbackStateDelta(id interface{}) error
	EmptyState() error
}
```

`WritableLedger`  interface allows for the caller to update the blockchain.  Note that this is _NOT_ intended for use in normal operation of a consensus plugin.  The current state should be modified by executing transactions using the `Executor` interface, and new blocks will be generated when transactions are committed.  This interface is instead intended primarily for state transfer or corruption recovery.  In particular, functions in this interface should _NEVER_ be exposed directly via consensus messages, as this could result in violating the immutability promises of the blockchain concept.  This interface is comprised of the following functions.

  -	```
	PutBlock(blockNumber uint64, block *pb.Block) error
	```

	This function takes a provided, raw block, and inserts it into the blockchain at the given blockNumber.  Note that this intended to be an unsafe interface, so no error or sanity checking is performed.  Inserting a block with a number higher than the current block height is permitted, similarly overwriting existing already committed blocks is also permitted.  Remember, this does not affect the auditability or immutability of the chain, as the hashing techniques make it computationally infeasible to forge a block earlier in the chain.  Any attempt to rewrite the blockchain history is therefore easily detectable.  This is generally only useful to the state transfer API.

  -	```
	ApplyStateDelta(id interface{}, delta *statemgmt.StateDelta) error
	```

	This function takes a state delta, and applies it to the current state.  The delta will be applied to transition a state forward or backwards depending on the construction of the state delta.  Like the `Executor` methods, `ApplyStateDelta` accepts an opaque interface `id` which should also be passed into `CommitStateDelta` or `RollbackStateDelta` as appropriate.

 -	```
	CommitStateDelta(id interface{}) error
	```

	This function commits the state delta which was applied in `ApplyStateDelta`.  This is intended to be invoked after the caller to `ApplyStateDelta` has verified the state via the state hash obtained via `GetCurrentStateHash()`.  This call takes the same `id` which was passed into `ApplyStateDelta`.

  -	```
	RollbackStateDelta(id interface{}) error
	```

	This function unapplies a state delta which was applied in `ApplyStateDelta`.  This is intended to be invoked after the caller to `ApplyStateDelta` has detected the state hash obtained via `GetCurrentStateHash()` is incorrect.  This call takes the same `id` which was passed into `ApplyStateDelta`.


  - ```
   EmptyState() error
   ```

	This function will delete the entire current state, resulting in a pristine empty state.  It is intended to be called before loading an entirely new state via deltas.  This is generally only useful to the state transfer API.

### 3.4.9 `RemoteLedgers` interface

Definition:

```
type RemoteLedgers interface {
	GetRemoteBlocks(peerID uint64, start, finish uint64) (<-chan *pb.SyncBlocks, error)
	GetRemoteStateSnapshot(peerID uint64) (<-chan *pb.SyncStateSnapshot, error)
	GetRemoteStateDeltas(peerID uint64, start, finish uint64) (<-chan *pb.SyncStateDeltas, error)
}
```

The `RemoteLedgers` interface exists primarily to enable state transfer and to interrogate the blockchain state at  other replicas.  Just like the `WritableLedger` interface, it is not intended to be used in normal operation and is designed to be used for catchup, error recovery, etc.  For all functions in this interface it is the caller's responsibility to enforce timeouts.  This interface contains the following functions.

  - ```
  GetRemoteBlocks(peerID uint64, start, finish uint64) (<-chan *pb.SyncBlocks, error)
  ```

	This function attempts to retrieve a stream of `*pb.SyncBlocks` from the peer designated by `peerID` for the range from `start` to `finish`.  In general, `start` should be specified with a higher block number than `finish`, as the blockchain must be validated from end to beginning.  The caller must validate that the desired block is being returned, as it is possible that slow results from another request could appear on this channel.  Invoking this call for the same `peerID` a second time will cause the first channel to close.

  - ```
   GetRemoteStateSnapshot(peerID uint64) (<-chan *pb.SyncStateSnapshot, error)
   ```

	This function attempts to retrieve a stream of `*pb.SyncStateSnapshot` from the peer designated by `peerID`.  To apply the result, the existing state should first be emptied via the `WritableLedger` `EmptyState` call, then the contained deltas in the stream should be applied sequentially.

  - ```
   GetRemoteStateDeltas(peerID uint64, start, finish uint64) (<-chan *pb.SyncStateDeltas, error)
   ```

	This function attempts to retrieve a stream of `*pb.SyncStateDeltas` from the peer designated by `peerID` for the range from `start` to `finish`.  The caller must validated that the desired block delta is being returned, as it is possible that slow results from another request could appear on this channel.  Invoking this call for the same `peerID` a second time will cause the first channel to close.

### 3.4.10 `controller` package

#### 3.4.10.1 controller.NewConsenter

Signature:

```
func NewConsenter(cpi consensus.CPI) (consenter consensus.Consenter)
```

This function reads the `peer.validator.consensus` value in `obc-peer/openchain.yaml` configuration file, which is the  configuration file for the `obc-peer` process. The value of the `peer.validator.consensus` key defines whether the validating peer will run with the `noops` consensus plugin or the `obcpbft` one. (Notice that this should eventually be changed to either `noops` or `custom`. In case of `custom`, the validating peer will run with the consensus plugin defined in `obc-peer/openchain/consensus/config.yaml`.)

The plugin author needs to edit the function's body so that it routes to the right constructor for their package. For example, for `obcpbft` we point to the `obcpft.GetPlugin` constructor.

This function is called by `helper.NewConsensusHandler` when setting the `consenter` field of the returned message handler. The input argument `cpi` is the output of the `helper.NewHelper` constructor and implements the `consensus.CPI` interface.

### 3.4.11 `helper` package

#### 3.4.11.1 High-level overview

A validating peer establishes a message handler (`helper.ConsensusHandler`) for every connected peer, via the `helper.NewConsesusHandler` function (a handler factory). Every incoming message is inspected on its type (`helper.HandleMessage`); if it's a message for which consensus needs to be reached, it's passed on to the peer's consenter object (`consensus.Consenter`). Otherwise it's passed on to the next message handler in the stack.

#### 3.4.11.2 helper.ConsensusHandler

Definition:

```
type ConsensusHandler struct {
	chatStream  peer.ChatStream
	consenter   consensus.Consenter
	coordinator peer.MessageHandlerCoordinator
	done        chan struct{}
	peerHandler peer.MessageHandler
}
```

Within the context of consensus, we focus only on the `coordinator` and `consenter` fields. The `coordinator`, as the name implies, is used to coordinate between the peer's message handlers. This is, for instance, the object that is accessed when the peer wishes to `Broadcast`. The `consenter` receives the messages for which consensus needs to be reached and processes them.

Notice that `obc-peer/openchain/peer/peer.go` defines the `peer.MessageHandler` (interface), and `peer.MessageHandlerCoordinator` (interface) types.

#### 3.4.11.3 helper.NewConsensusHandler

Signature:

```
func NewConsensusHandler(coord peer.MessageHandlerCoordinator, stream peer.ChatStream, initiatedStream bool, next peer.MessageHandler) (peer.MessageHandler, error)
```

Creates a `helper.ConsensusHandler` object. Sets the same `coordinator` for every message handler. Also sets the `consenter` equal to: `controller.NewConsenter(NewHelper(coord))`

### 3.4.11.4 helper.Helper

Definition:

```
type Helper struct {
	coordinator peer.MessageHandlerCoordinator
}
```

Contains the reference to the validating peer's `coordinator`. Is the object that implements the `consensus.CPI` interface for the peer.

#### 3.4.11.5 helper.NewHelper

Signature:

```
func NewHelper(mhc peer.MessageHandlerCoordinator) consensus.CPI
```

Returns a `helper.Helper` object whose `coordinator` is set to the input argument `mhc` (the `coordinator` field of the `helper.ConsensusHandler` message handler). This object implements the `consensus.CPI` interface, thus allowing the plugin to interact with the stack.


#### 3.4.11.6 helper.HandleMessage

Recall that the `helper.ConsesusHandler` object returned by `helper.NewConsensusHandler` implements the `peer.MessageHandler` interface:

```
type MessageHandler interface {
	RemoteLedger
	HandleMessage(msg *pb.OpenchainMessage) error
	SendMessage(msg *pb.OpenchainMessage) error
	To() (pb.PeerEndpoint, error)
	Stop() error
}
```

Within the context of consensus, we focus only on the `HandleMessage` method. Signature:

```
func (handler *ConsensusHandler) HandleMessage(msg *pb.OpenchainMessage) error
```

The function inspects the `Type` of the incoming `OpenchainMessage`. There are four cases:

  1. Equal to `pb.OpenchainMessage_CONSENSUS`: passed to the handler's `consenter.RecvMsg` function.
  2. Equal to `pb.OpenchainMessage_CHAIN_TRANSACTION` (i.e. an external deployment request): a response message is sent to the user first, then the message is passed to the `consenter.RecvMsg` function.
  3. Equal to `pb.OpenchainMessage_CHAIN_QUERY` (i.e. a query): passed to the `helper.doChainQuery` method so as to get executed locally.
  4. Otherwise: passed to the `HandleMessage` method of the next handler down the stack.

### 3.5 Events
### 3.4.1 Event Stream
### 3.4.2 Event Structure
### 3.4.3 Event Adapters


## 4. Security

For this section we will be considering the setting depicted in Figure I. In particular, we identify the following entities:
 * Membership management infrastructure, i.e., a set of entities that are responsible for identifying an individual user (using any form of identification considered in the system, e.g., credit cards, id-cards), open an account for that user to be able to register, and issue the necessary credentials to successfully create transactions and deploy or invoke chain-codes successfully through Open Blockchain.

 * Peers, that we classify in validating peers, and non-validating peers. Validating peers (also known as validators), are the entities that order and process (check validity and execute & add to the blockchain) user-messages (transactions) submitted to the network on behalf of users. Non validating peers (also known as peers) receive user transactions on behalf of users, and after doing some basic validity checks, they forward the transactions to their neighboring validating peers. Peers maintain an up-to-date copy of the blockchain, but in contradiction to validators, they do not execute transactions (a process also known as *transaction validation*).
 * End users of the system, that have registered to our membership service administration, after having demonstrated ownership of what is considered *identity* in the system, and have obtained credentials to install the client-software and contribute transactions to the system.
 * Client-software, the software that needs to be installed at the client side for the latter to be able to complete his registration to our membership service and submit transactions to the system.
 * Online wallets, entities that are trusted by a user to maintain that user's credentials, and submit transactions solely upon user request to the network. Online wallets come with their own software at the client-side, that is usually light-weight, as the client only needs to authenticate himself and his requests to the wallet. While it can be the case that peers can play the role of *online wallet* for a set of users, in the following sessions we will investigate the behavior and security of online wallets separately.

Users who wish to make use of Open Blockchain, open an account at the membership management administration, by proving ownership of as discussed in previous sections, new chain-codes are announced to the Blockchain by the chain-code creator (developer) through the means of a deployment transaction that the client-software would construct on behalf of the developer. Such transaction is first received by a peer or validator, and afterwards circulated in the entire network of validators, this transaction is executed and finds its place to the Blockchain. Users can also invoke a function of an already deployed chain-code through an invocation transaction.
In the following we provide an overview of the desired business security requirements w.r.t. our system, and how do these map to our security goals. We then overview the security components and their operation and show how our design achieves the prescribed goals.


### 4.1 Business security requirements
#### 4.1.1 Preserving user privacy while incorporating identity and role management
In order to adequately support real business applications and scenarios it is necessary to progress beyond ensuring “cryptographic continuity” (where, as in Bitcoin for example, outputs are to one or more addresses / hashes of public keys so as to require knowledge of the corresponding private keys in order to further transact). A workable B2B system must consequently move towards addressing proven/demonstrated identities or other attributes relevant to conducting business. Even within the consumer space, digital currency that is convertible back into standard monetary instruments requires a degree of identity management in order to provide accountability to detect and trace money laundering. It is insufficient to revoke anonymity only under conditions of double spending. Business transactions and consumer interactions with financial institutions need to be unambiguously mapped to accountholders. Business contracts typically require demonstrable affiliation with specific institutions and/or possession of other specific properties of transacting parties.

The approach taken here to reconcile identity management with user privacy and to enable competitive institutions to transact effectively on a common blockchain (for both intra- and inter- institutional transactions) is as follows:

1. add certificates to transactions to implement a “permissioned” blockchain
2. utilize a two-level system:
  1. (relatively) static enrollment certificates, acquired via registration with an enrollment certificate authority (CA).
  2. transaction certificates (TCerts) that faithfully but pseudonymously represent enrolled users, acquired via a transaction CA.

An enrollment certificate includes an enrollment ID (or EnrollID). Such enrollment ID may incorporate actual user identity and/or institutional affiliation(s), roles, etc. (e.g., demonstrated or proven by the user via some established out-of-band means). If one uses an enrollment certificate directly as part of a transaction to meet a requirement of attestation of, say, institutional affiliation, even if such information is hashed, encrypted or otherwise hidden within the formulation of the enrollment certificate, use of the enrollment certificate across transactions will leak common ownership. Instead, one can use TCerts generated by a Transaction Certificate Authority (TCA) that extracts information from the enrollment certificate (where such information can be encrypted within the TCert to enable control of to which entities such information is visible). Through proper use of TLS, enrollment certificates need not be exposed to eavesdroppers, for example, during their delivery to clients or their transmission from clients to the TCA for TCert batch requests. Batch requests are signed using the private key that corresponds to the subject public key within the enrollment certificate.

The enrollment certificate acquisition process should be renewable in order to accommodate changes such as to roles or affiliations reflected by enrollment ID. Aspects relevant to a user’s profile as reflected within TCerts may vary from one batch of TCerts to the next.

TCerts can accommodate encryption or key agreement public keys as well as digital signature verification public keys, whether within a TCert itself or as an extension of a TCert as follows:
The entity may use TCertPriv_Key corresponding to TCertPub_Key within a TCert to digitally sign a chain code/transaction content that includes, in particular, an encryption public key or key agreement public key for which the entity has or can retrieve the corresponding decryption private key or key agreement private key. The entity may use a TCert to disseminate on the blockchain an encryption public key or key agreement public key that is usable by the entity or another entity to make a key accessible to encrypt data. The key that is used to encrypt data is made accessible by encrypting that key using the encryption public key or by encrypting that key using a key that is derived from a shared secret value that results from using the key agreement public key in a suitable key agreement protocol. This enables an entity to decrypt the encrypted data in order to recover the plaintext form, i.e., usable form, of the data. Such encryption public key or key agreement public key can be generated using a method that is the same as or similar to that used to generate the TCertPub_Key, using an index value that is the same as or related to the TCertIndex value within the TCert that accompanies the signed chain code/transaction content. Rather than generating such key agreement public key as (EnrollPub_Key
+ Expansion Value G), where Expansion Value is determined from Expansion_Key = HMAC(TCertOwnerKDF_Key, “2”) and some value of TCertIndex, such key agreement public key can be generated as follows: In place of EnrollPriv_Key and corresponding EnrollPub_Key, the entity may use an independently generated long-term private key and corresponding long-term public key. In place of Expansion_Key = HMAC(TCertOwnerKDF_Key, “2”), the entity may use an independently generated long-term HMAC key. Alternatively, EnrollPriv_Key and corresponding EnrollPub_Key and Expansion_Key can be reused for this purpose if, for example, the TCA uses only odd values for the TCertIndex counter when generating tokens with TCerts so that the even values of TCertIndex are reserved for the key agreement private and public keys, such as elliptic curve Diffie-Hellman (ECDH) private and public keys. This technique enables the entity to reconstruct the key agreement private key by viewing the transaction on the block chain where, for example, the index used for the ECDH key is one higher than the TCertIndex of the TCert. More generally, this technique or a similar technique enables the entity to reconstruct the decryption private key or key agreement private key by viewing the transaction on the block chain. As an example, two entities that want to communicate with one another as determined based on the attributes and/or reputation scores, which may appear in the clear within the TCerts of each, and want to communicate securely using encrypted data, may use dissemination on the block chain of an encryption public key or key agreement public key. As another example, where a first of two entities is aware by some means of an encryption key or key agreement public key of the second of the two entities, the first entity may use the encryption key or key agreement public key of the second entity (here, the intended recipient) to securely provide access to the first entity’s attributes, reputation scores, EnrollID, or any other data of interest, so that the second entity can decide whether to communicate with the first entity, where such communications may occur on and/or off of the blockchain. If the second entity decides to communicate with the first entity, the second entity can do so using the encryption public key or key agreement public key that was provided by the first entity within the transaction that provided the second entity with access to the first entity’s attributes, reputation scores, EnrollID, or any other data of interest.

#### 4.1.2 Supporting invocation access control without precluding privacy

Resistance against traffic analysis, even relative to iterative invocations of the same deployment transaction by the same user, can be accomplished by using a different TCert per invocation. Under encryption for confidentiality, use the private key of a TCert that was included within the referenced deployment transaction’s access control list (ACL), which is distinct from the private key of the newly introduced TCert, in order to prove authorization to invoke. This method can apply whether or not chaincode within the deployment transaction and/or inputs for invocation require confidentiality.

Whether or not encrypted for confidentiality so as to provide resistance against traffic analysis, the method above can be used to address the case that the TCert within the ACL has expired by the time that invocation occurs. The newly introduced TCert must be currently valid.

In the case of access control that is not user-specific, but rather only requires, e.g., affiliation with a specific institution, there is no need to acquire TCerts of intended authorized invokers for incorporation into an ACL. This case easily lends itself to protecting against traffic analysis by having users present a different TCert each time, as long as each such TCert carries the appropriate access criterion/criteria, such as institution affiliation specified by the referenced deployment transaction.

_TODO: add figure to illustrate the two cases above as in the two examples below._

**Example 1:**

Invocation Transaction: TCert<sub>new</sub>, Sign<sup>1</sup>(Ciphertext Transaction), Ciphertext Transaction

where Plaintext Transaction includes, in particular, TCert<sub>acl</sub>, Sign<sup>2</sup>(Initial Plaintext || hash(TCert<sub>new</sub>)), and Initial Plaintext

<sup>1</sup><sub>signature computed using private key corresponding to TCert<sub>new</sub></sub>

<sup>2</sup><sub>signature computed using private key corresponding to TCert<sub>acl</sub>, where TCert<sub>acl</sub> is one of the TCerts from the referenced Deployment Transaction</sub>


**Example 2:**

Deployment Transaction specifies invocation criteria, e.g., affiliation with Institution A

Invocation Transaction signed using TCert, where TCert specifies affiliation with Institution A. Another such TCert (from the same batch or a different batch) is used the next time, if any, the same user invokes.

#### 4.1.3 Addressing reputation/risk management

There is a marked contrast of potential risk undertaken in a system that does not have adequate safeguards in place, particularly when comparing limited loss of funds in a digital currency-only system vs. irreparable harm to reputation of users and/or corporations in a general purpose B2B system.

In Open Ledger, the (persistent) enrollment private key is used in two ways, namely to

1. sign a request for a batch of TCerts, and
2. derive the TCert-unique TCert private keys from the TCerts and from the value of the key derivation function / HMAC key, TCertOwnerKDF_Key, that is delivered by the TCA with each batch of TCerts.

Countermeasure/containment against compromise of an enrollment private key: Multi-signature can be applied at the enrollment certificate level for TCert batch requests. In this case there would be two enrollment certificates associated with each user, and TCert batch requests would be required to be signed twice independently, using the enrollment private key corresponding to each of the two enrollment public keys. This would not affect the issued TCerts if those are structured based on only one of the two enrollment certificates. However, the TCerts could be structured to enable enforcement of active involvement of both enrollment private keys on a per-transaction basis. One way to do this is to include two TCert public keys in each TCert (based on each of the two enrollment public keys, respectively). A replying party would then expect to see two signatures on each transaction that includes such a TCert, each verifiable using one of the two TCert public keys.

Alternatively, TCerts could be issued in pairs based on each of the two enrollment public keys.

Roles/attributes/authorizations and/or other user profile properties of the like can be incorporated directly into TCerts (whether in plaintext or ciphertext form), independently of whether such user profile properties are available for extraction from an enrollment certificate. There may be a requirement for in-band or out-of-band proof of possession of such properties in order to satisfy a TCA (or a user-facing registration authority that resides in front of a TCA) that a batch of TCerts that each include such properties should be issued. A TCert may furthermore include one or more reputation and/or risk management scores relevant to one or more properties of the TCert.

In addition to the basic TCert fields included in the TCert structure, additional fields may be included. In order to enable a token with TCert to optionally include means to allow targeted release of certain data fields incorporated into TCert, a 384-bit TCertInfoExportKDF_Key can be generated as HMAC(TCertOwnerKDF_Key, “3”), where TCertOwnerKDF_Key is delivered with each batch of TCerts, and “3” is some representation of the integer 3 as distinct from “1” and “2”. HMAC(TCertOwnerKDF_Key, “3”) can be used to generate one or more keys, each of which can be used to encrypt one or more data fields. For example, a 256-bit EncryptEnrollID_Key derived as [HMAC(TCertInfoExportKDF_Key, TCertID || “1”)]<sub>256-bit truncation</sub> can be used for targeted release of EnrollID if EnrollID appears within a TCert as AES_Encrypt<sub>EncryptEnrollID_Key</sub>(EnrollID), that is, as an encryption of the EnrollID value where such encryption is executed using the advanced encryption standard (AES) algorithm with key EncryptEnrollID_Key. Here EnrollID can be extracted or derived by the TCA from the enrollment certificate of the TCert owner. The TCA and/or appropriate delegate may wish to look at specific transactions (e.g., in order to obtain a reputation score included in a TCert, in order to obtain an attribute included in a TCert, etc.) or to link specific transactions, e.g., transactions involving a given entity or set of entities. For example, the TCA may wish to look at specific transactions in order to compile a reputation score and/or attribute(s) by examining a series of transactions submitted by or otherwise involving the entity as a party to a transaction. Attributes and/or reputation scores may be incorporated into a TCert as AES_Encrypt<sub>EncryptAttributes_Key</sub>(Attribute(s) || Reputation Score(s)), where, for example, EncryptAttributes_Key is generated as [HMAC(TCertInfoExportKDF_Key, TCertID || “2”)]<sub>256-bit truncation</sub>. Here TCertID may be unique per TCert, which would render instances of EncryptAttributes_Key (and therefore AES_Encrypt<sub>EncryptAttributes_Key</sub>(Attribute(s) || Reputation Score(s)) for the same Attribute(s) || Reputation Score(s)) distinct. The TCA and/or appropriate delegate(s) retains or has access to EncryptAttributes_Key, as does the TCert owner using the TCert in association with the transaction(s)), and where EncryptAttributes_Key can be made available to additional entities. For example, EncryptAttributes_Key can be made available within a transaction that is signed using an entity’s private key, i.e., TCertPriv_Key, as an encryption that provides EncryptAttributes_Key to the targeted entities via decryption using an appropriate key. Attributes and/or reputation scores may alternatively be provided in unencrypted form, e.g., where attributes are not considered sensitive information and attribute-reputation score combinations do not enable unwanted clustering by observers according to the same TCert owner. A reputation score may be relative to specific attribute(s).

If a field of the form, for example, AES_Encrypt<sub>TCA_Encrypt_Key</sub>(TCertIndex || EnrollPub_Key || EnrollID), for TCA_Encrypt_Key known by the TCA, is included within the TCert by the TCA, the TCA can later identify the TCert owner (via EnrollID) from a transaction that is associated with such TCert. The TCA can directly access the attributes and reputation scores (although these may be in encrypted form within the TCerts) if the TCerts are placed outside of the boundary of the encrypted chain code. Although the attributes and/or reputation scores may be in encrypted form within the TCerts, the TCA and/or its delegates can access the plaintext form of such attributes and/or reputation scores because of the ability to access the key used for the encryption. Additionally, If attributes and reputation scores (or other data of interest) are incorporated into TCerts, it may become reasonable for the TCA (or whichever entity that is responsible for assigning reputation scores) to be granted access to plaintext chain code.

#### 4.1.4 Enabling a system transactions capability

Rather than relegating certain critical functions to be handled out-of-band of a particular blockchain, these can be addressed via the use of “system transactions.” Whether such system transactions are deployed at initially unspecified points or within a genesis block, their deployment and subsequent invocation need not necessarily employ specialized access control mechanisms that address conditions of commitment to the blockchain. Analogously to colored coins, relying parties relative to these transactions can be equipped with the means to process these transactions within the appropriate context. Read-access of portions of these transactions that may remain opaque to all but a select group of entities can be (implicitly) controlled through the proper use of wrapping keys. Similarly, specific aspects of execution can be reserved for authorized entities without necessarily handling enforcement at the general validation level. For example, suppose that setting (i.e., expiration and/or initialization) of Validity Period is attempted through invocation of a system transaction. Correct assessment of current Validity Period is relied upon by validators that determine whether or not a Transaction Certificate (TCert) that is used for deployment or invocation should be considered to be currently legitimate. While it may be not be considered detrimental to commit an invocation transaction to the blockchain that purports to set Validity Period, if such invocation transaction was based on a TCert it should be rejected as not affecting Validity Period. Even if transaction replay protection is adequately addressed at the general validation level, the transaction’s effect on setting Validity Period should only be accepted if it was signed using a suitable key, such as the private key of the Transaction Certificate Authority (TCA) that corresponds to the subject public key within a valid TCA Certificate rather than a private key that corresponds to a TCert public key. Similarly, an invocation transaction that attempts to update a particular type of certificate revocation list (CRL) should be rejected by relying parties unless the transaction has been signed using the private key that corresponds to the subject public key of an authority deemed responsible for such CRL. For example, the TCA relies on Enrollment Certificates of requesters of batches of TCerts. Consequently, updates to an Enrollment Certificate CRL should only be accepted by the TCA if they are signed by the ECA (using the private key corresponding to the subject public key of a currently valid ECA Certificate) or an entity explicitly delegated by the ECA for such purpose. Another potential use of (suitably encrypted) system transactions is the dissemination of properly sourced random inputs to be used during accounts processing, such as to disguise against eavesdropping which row(s) of a database are modified to actual effect by any given blockchain transaction (through innocuous transformation such as via random/pseudo-random permutation, account splitting, and/or noise injection).

The following method can be used to securely handle system transactions by taking advantage of the fact that legitimate state updates follow a predictable pattern, such as monotone increasing or monotone decreasing. For example, an updated Validity Period should be higher or more recent than the Validity Period it supersedes. Similarly, the version number of a CRL update should be higher than its predecessor update. If such a rule/constraint on legitimate update is incorporated into the deployment transaction that is referenced by invocation transactions, then this can be checked against the appropriate state table entries as a condition of update without any dependency on replay protection/detection at the transaction level. Therefore, the invocation transaction can be signed by using the private key corresponding to the subject public key of an arbitrary TCert. The appropriate rule(s) governing invocation are incorporated into the deployment transaction, and are available for use during execution of the chaincode of the invocation transaction. One such rule could be that the state update request within the invocation chaincode is signed (internally to the transaction) using the public key of an appropriate authority. For example, the deployment transaction for Validity Period update may incorporate a hash of a TCA certificate, so that only invocation based on use of that TCA’s private key is accepted as eligible to update state. More generally, where there may be more than one TCA certificate or a TCA certificate might be issued later by an appropriate entity higher in the PKI hierarchy, the deployment transaction may include a Certificate Type, where successful invocation requires that the state update request internal to an invocation transaction is signed using a private key that corresponds to a subject public key contained within a certificate of the required Certificate Type.

#### 4.1.5 Flexibly accommodating usage of transaction certificates

Synopsis: The design is capable of enabling auditability, and client reliability and portability –- where full TCerts (not just their hashes) of those users who are eligible to invoke are incorporated into deployment transactions, and TCerts include a field decryptable by auditors as well as a field decryptable by the particular TCert owner / user’s client. The set of auditors that can decrypt specified field(s) of TCerts may vary from one batch of TCerts to the next.

If a client retains or can retrieve or regenerate the hash of each of its issued TCerts, then it can do a compare against this list to determine if a given hashed TCert in an ACL belongs to that user. If knowledge of that TCert and its private key is required to invoke, then the actual TCert needs to be available to the client, either via locally accessible storage or via its inclusion within the deployment transaction. Given access to a TCert, there are at least two ways for a client to determine whether or not it belongs to a user of that client without the need to store a representation of each of its individual TCerts:

1. The initial TCertIndex of each batch as retained after decrypting

   AES_Encrypt<sub>TCertOwnerEncrypt_Key</sub>(TCertIndex) from the first TCert within the batch is sufficient to compute

   AES_Encrypt<sub>TCertOwnerEncrypt_Key</sub>(TCertIndex) for all TCerts within the batch. TCertOwnerEncrypt_Key is derivable from TCertOwnerKDF_Key that is included with each batch, e.g., as [HMAC(TCertOwnerKDF_Key, “1”)]<sub>256-bit truncation</sub>
2. If within the TCert some appropriate function of the enrollment certificate, such as enrollment public key or enrollment ID, is included along with TCertIndex under the AES encryption using TCertOwnerEncrypt_Key, then the client can compare that field against that of its user(s) to determine whether the client owns that TCert. Note that

   AES_Encrypt<sub>TCertOwnerEncrypt_Key</sub>(TCertIndex) (with or without extension under the encryption to include a function of the enrollment certificate) is sufficient for the client to rederive the TCert private key, as long as it retains or has access to the enrollment private key and TCertOwnerKDF_Key.

   This can be done as TCertPriv_Key = (EnrollPriv_Key + ExpansionValue) modulo n, where 384-bit ExpansionValue = HMAC(Expansion_Key, TCertIndex) and 384-bit Expansion_Key = HMAC(TCertOwnerKDF_Key, “2”). The corresponding TCertPub_Key expression is TCertPub_Key = EnrollPub_Key + ExpansionValue G.

If a similar field is included within a TCert, say, AES_Encrypt<sub>K</sub>(TCertIndex || fcn(enrollment certificate)) that is generated by encrypting using a key K made available to an auditor, then that auditor can, in particular, determine which transactions have a particular entity in common (at least relative to those batches of TCerts for which the auditor has been given access to a suitable key K). Note that even if the auditor has no need to see the value of TCertIndex, its inclusion here prevents unintended repetition of the same value across TCerts.

#### 4.1.6 Expanding to support root CA functionality  

Although the initial code utilizes self-signed certificates for entities such as the enrollment CA (ECA) and the transaction CA (TCA), it is ultimately desirable to implement a full PKI based on a root CA, and possibly intermediate CAs (such as a CA responsible for issuing certificates to multiple ECAs, and/or a CA responsible for issuing certificates to multiple TCAs). One benefit of a full PKI is the ability to handle revocation of entities (with the exception of the root CA itself). There can, furthermore, be cross-certification across chains with distinct root CAs.

### 4.2 User Privacy through Membership Services

A Public Key Infrastructure is a set of tools, entities, policies, etc. that use digital certificates to manage public-key encryption. The Membership Service infrastructure comprises the following entities: Enrollment Certificate Authority (ECA), Transaction Certificate Authority (TCA) and TLS Certificate Authority (TLS-CA). In this specification, membership services is expressed through following associated certificates issued by the PKI:  

1. Enrollment Certificates (ECerts) – long-term certificates for each user
2. Transaction Certificates (TCerts) - short-term certificates for each transaction and
3. TLS-Certificates (TLS-Certs) – certificates used for system 2 system communication

* Enrollment Certificate Authority (ECA): entity that generates Enrollment Certificates (ECerts) that include signature certification and encryption keys.  ECerts contain the identity of their owner and can be used to offer only nominal entity-authentication in transactions. ECerts are issued to all roles, i.e. user, non-validating peers and validating peers. They contain the public part of two key pairs – a signature key-pair and an encryption key-pair. ECerts are accessible to everyone.

* Transaction Certificate Authority (TCA): entity that generates TCerts.  A TCert securely authorizes a transaction without revealing the identities of who is involved in the transaction. In this implementation TCerts do not carry information of the identity of the user. They enable the user not only to anonymously participate in the system but also prevent linkability of transactions. However, auditbility and accountability requirements assume that the TCA is able to retrieve TCerts of a given identity, or retrieve the owner of a specific TCert.

* TLS Certificate Authority: Issues TLS certificates for the system participants. Peers (validating and non-validating peers) use TLS certificates.  They carry the identity of their owner and are used for network level security.

The structure of a Transaction Certificate (TCert) is as follows:
*	TCertID – transaction certificate ID
*	TCertPub_Key – TCert public key
*	KeyDF_Key  - Key-Derivation-Function Key
*	Validity period – the time window during which the certificate can be used
*	AES_Encrypt <sub>TCertOwner_EncryptKey</sub> (TCertIndex)  

This implementation of membership services provide the following basic functionality: there is no expiration/revocation of ECerts; expiration of TCerts is provided via the validity period time window; there is no revocation of TCerts. The ECA, TCA, and TLS CA certificates are self-signed, where the TLS CA is provisioned as a trust anchor. (Comment – maybe we could add that that alternately a full PKI could be used where a root CA and potentially intermediate CAs are used)


#### 4.2.1 User/Client Enrollment Process

The Registration Authority (RA) is a trusted entity that can ascertain the validity and identity of users who want to participate in the permissioned blockchain. As illustrated in Figure 1, (Fig1: User Enrollment Process) the user enrollment process is executed in two phases:

*Offline Phase:* the user presents strong identification credentials (proof of ID) to a Registration Authority (RA) offline. The RA creates and stores an account for the user. The RA returns the associated username/password and Root Certificate (TLS-CA Cert in this implementation) to the user.

*Online Phase:*  the user contacts the ECA sending his username, password and a certificate request. The user sends to the ECA its public key and additional identity information. This information in turn is signed by the ECA. The ECA returns to the client an enrollment certificate and the encryption key of the chain. The client saves in local storage both certificates. At this point the user enrollment has been completed.


### 4.3 Transaction security offerings at the infrastructure level

Transactions in Open Blockchain are user-messages submitted to be included in the ledger. These messages have a specific structure, and enable users to deploy new chain-codes, invoke existing chain-codes, or query the state of existing chain-codes. Therefore, the way transactions are formed, announced and processed plays an important role to the privacy and security offerings of the entire system.

On one hand our membership service provides us with the means to authenticate transactions as having originated by valid users of the system, to disassociate transactions with user identities, but while efficiently tracing the transactions a particular individual under certain conditions (law order, auditing). In other words, membership services offer to transactions authentication mechanisms that marry user-privacy with accountability and non-repudiation.

On the other hand, membership services alone cannot offer full privacy  of user-activities within
Openblockhain. First of all, for privacy provisions offered by Open Blockchain to be complete,
privacy-preserving authentication mechanisms need to be accompanied by transaction confidentiality.
This becomes clear if one considers that reading a chain-code, may  leak information on who may have
created, and thus break the privacy of that chain-code's creator. Section 5.1 elaborates on this.

Enforcing access control on the invocation of chain-codes is another part of the security of the chain-code, that requires special care. Though for this one can leverage authentication mechanisms of membership services, one would need to design invocation ACLs and perform their validation in a way that non-authorized parties cannot link multiple invocations of the same chain-code by the same user. Section 5.2 elaborates on this.

Finally, replay attacks is another crucial aspect of the security of the chain-code, as a malicious user
may try to copy a transaction that was added to the Blockchain long time ago, and replay it in the network
to distort its operation. This is the topic of Section 5.3.

### 4.4 Transaction Confidentiality

Transaction confidentiality requires that under the request of the developer, the plain-text of a chain-code, i.e., code, description, is not accessible or inferable (assuming a computational attacker) by any entity not properly authorized by the developer of the chain-code (user or peer). For the latter it is important that for contracts with confidentiality requirements the content of both deployment and invocation transactions remains concealed, and all of them unlinkable to each to other as transactions associated to the same chain-code.

Additional requirements for any candidate solution is that it respects and supports the privacy and security provisions of the underlying membership service. In addition, it should not prevent the enforcement of any invocation access control of the chain-code functions in the fabric, or the implementation of enforcement of access-control mechanisms on the application (See Section 7).

We target a design that will allow for granting or restricting access to an entity to any subset of the following parts of a chain-code:
1. chain-code content, i.e., complete (source) code of the chain-code,
2. chain-code function headers & ACLs, i.e., the prototypes of the functions included in a chain-code, and their respective list of (anonymous) identifiers of users who should be able to invoke each function (*)
3. chain-code [invocations &] state, i.e., successive updates to the state of a specific chain-code, when one or more functions of its are invoked
4. all the above

Notice, that we aim to offer the capability to the application to leverage our PKI & blockchain infrastructure to build their own access control policies and enforcement mechanisms.

To support finer-grain confidentiality, i.e., restrict read-access to the plain-text of a chain-code to a subset of users that the chain-code creator defines, we move to the public key setting for encryption.
Here a chain is to be bound to a single long�term encryption key-pair (PK<sub>chain</sub>, SK<sub>chain</sub>) [as opposed to the symmetric encryption key Kchain, that we use in the first release].
Initially, this key-pair is to be stored and maintained by each chain's PKI. In later releases, however, we can move away from this restriction, as chains (and the associated key-pairs) can be triggered through
the Blockchain by any user with *special* (admin) privileges (See, multi-chain sub-section).
At enrollment phase, user obtain (as before) an enrollment certificate, denoted by Cert<sub>u<sub>i</sub></sub> for user u<sub>i</sub>, while each validator v<sub>j</sub> obtain its enrollment certificate denoted by
Cert<sub>v<sub>j</sub></sub>. Entity enrollment would be enhanced, as follows.

1. Users
   a. claim and grant themselves encryption key-pair (epk<sub>u</sub>, esk<sub>u</sub>), (already present in the first release)
   b.obtain the encryption (public) key of the chain PK<sub>chain</sub>
2. Validators
   a. claim and grant themselves an encryption key-pair (epk<sub>v</sub>, esk<sub>v</sub>)
   b. obtain the decryption (secret) key of the chain SK<sub>chain</sub>

Thus, enrollment certificates contain the public part of two key-pairs:
* one signature key-pair [denoted by (spk<sub>v<sub>j</sub></sub>,ssk<sub>v<sub>j</sub></sub>) for validators and by (spk<sub>u<sub>i</sub></sub>, ssk<sub>u<sub>i</sub></sub>) for users], and
* an encryption key-pair [denoted by (epk<sub>v<sub>j</sub></sub>,esk<sub>v<sub>j</sub></sub>) for validators and (epk<sub>u<sub>i</sub></sub>, esk<sub>u<sub>i</sub></sub>) for users]

Chain, validator and user enrollment public keys are accessible to everyone.

In addition to enrollment certificates, users who wish to anonymously participate in transactions issue transaction certificates.
For simplicity transaction certificates of a user u<sub>i</sub> are denoted by TCert<sub>u<sub>i</sub></sub>. Transaction certificates
include the public part of a signature key-pair denoted by  (tpk<sub>u<sub>i</sub></sub>,tsk<sub>u<sub>i</sub></sub>).

In the following we detail how the transaction format is enhanced to accommodate read-access restrictions at the level of a user.

### 4.5 Deployment transaction
Figure 5 depicts the structure of a typical deployment transaction with confidentiality enabled.

![FirstRelease-deploy](./images/sec-futrel-depl.png)

One can notice that a deployment transaction consists of several sections:
* section *general-info*, that contains the administration details of the transaction, i.e., which chain this transaction corresponds to (chained), the type of transaction (that is set to ''deploymetTx''), the version number of confidentiality policy implemented, its creator identifier (expressed by means of transaction certificate TCert of enrollment certificate Cert), and a Nonce, that facilitates primarily replay-attack resistance techniques.
* section *code-info*, that contains information on the chain-code source code, function headers, (invocation) access list, etc. In the version of the transaction format depicted in Figure 5, there is a symmetric key used for the source-code of the chain-code (K<sub>C</sub>), and another symmetric key used for the function prototypes and ACLs (K<sub>H</sub>). A hash of the headers (function-prototypes & ACL) is computed and added in the source-code section, such that the latter is uniquely associated with the former.
* section *chain-validators*, where appropriate key material is passed to the validators for the latter to be able to (i) decrypt the chain-code source (K<sub>C</sub>), (ii) decrypt the headers and ACLs(K<sub>H</sub>),  and (iii) encrypt the state when the chain-code has been invoked accordingly(K<sub>S</sub>). In particular, the chain-code creator generates an encryption key-pair for the chain-code it deploys (PK<sub>C</sub>, SK<sub>C</sub>). It then uses PK<sub>C</sub> to encrypt all the keys associated to the chain-code:
<center> [(''code'',K<sub>C</sub>) ,(''headr'',K<sub>H</sub>),(''code-state'',K<sub>S</sub>), Sig<sub>TCert<sub>u<sub>c</sub></sub></sub>(\*)]<sub>PK<sub>c</sub></sub>, </center>
  and passes the secret key SKC to the validators using the chain-specific public key:
<center>[(''chaincode'',SK<sub>C</sub>), Sig<sub>TCert<sub>u<sub>c</sub></sub></sub>(*)]<sub>PK<sub>chain</sub></sub>.</center>
* section *contract-users*, where the public encryption keys of the contract users, i.e., users who are given read-access to parts of the chain-code, are used to encrypt the keys  associated to their access rights:

  1. SK<sub>c</sub> for the users to be able to read any message associated to that chain-code (invocation, state, etc),
  2. K<sub>C</sub> for the user to be able to read only the contract code,
  3. K<sub>H</sub> for the user to only be able to read the headers and ACLs,
  3. K<sub>S</sub> for the user to be able to read the state associated to that contract. Finally users are given the contract�s public key PK<sub>c</sub>,

 for them to be able to encrypt information related to that contract for the validators (or any in possession of SK<sub>c</sub>) to be able to read it. Transaction certificate
 of each contract user is appended to the transaction and follows that user's message. This is done for users to be able to easily search the blockchain
 for transactions they have been part of. Notice that the deployment transaction also appends a message to the creator u<sub>c</sub> of the chain-code, for the
 latter to be able to retrieve this transaction through parsing the ledger and without keeping any state locally.


The entire transaction transaction is signed by the transaction/enrollment certificate of the chain-code creator (as decided by the latter).
Two noteworthy points:
* The cleartext of all messages that are finally encrypted under some key (symmetric or asymmetric)
  inside the transaction type, is also signed using the same TCert the ciphertext is encrypted with,
  and is attached to the transaction. [Comment the case where a client signes the Tx with a TCert and the plaintext with an ECert.].
  The latter is done such that mix\&match attacks are not possible, i.e., an attacker who sees a transaction cannot
  isolate the ciphertext corresponding to, e.g., code-info, and use it for another transaction of her own. Clearly,
  such an ability would disrupt the operation of the system, as a chain-code that was first created by user A,
  will now also belong to malicious user B (who is not even able to read it).
* Ciphertexts that are encrypted with a key K are accompanied by a (Pedersen) commitment to K, while the opening
  of this commitment value is passed to all users who are entitled access to K in contract-users, and chain-validator sections.
  In this way, anyone who is entitled access to that key can verify that the key has been properly passed to it.
  This part is omitted in the description of Figure 5 to avoid confusion.



### 4.6 Invocation transaction
A transaction invoking the chain-code triggering the execution of a function of the chain-code with user-specified arguments
is structured as depicted in Figure 6.

![FirstRelease-deploy](./images/sec-futrel-inv.png)

Invocation transaction as in the case of deployment transaction consists of a *general-info* section, a *code-info* section, a section for the *chain-validators*, and one for the *contract users*,
signed altogether with one of the invoker user's transaction certificates.

General-info follows the same structure as the corresponding section of the deployment transaction.
The only difference relates to the transaction type that is now set to ''InvTx'', and the chain-code identifier or name that is now encrypted under the chain-specific encryption (public) key.

Code-info exhibits the same structure as the one of the deployment transaction. Code payload, as in the case of deployment transaction, consists of function invocation details
(the name of the function invoked, and associated arguments), code-metadata provided by the application, and the transaction's creator (invoker's u<sub>i</sub>) certificate, TCert<sub>u<sub>i</sub></sub>'.
Code payload is signed by another transaction certificate TCert<sub>u<sub>i</sub></sub> of the invoker u<sub>i</sub>, and serves invocation access control enforcement purposes. The latter is
detailed in the next Subsection. As in the case of deployment transactions, code-metadata, and tx-metadata, are fields that are provided by the application and can be used (as described in Section 7), to implement their own ACLs.

Finally, contract-users and chain-validator sections provide the key the payload is encrypted with under the invoker�s key, and the chain encryption key respectively.
Upon receiving such transactions, the validators, decrypt [code-name]<sub>PK<sub>chain</sub></sub> using the chain-specific secret key SK<sub>chain</sub> and obtain the invoked chain-code identifier.
Given the latter, validators retrieve from their database the chain-code's decryption key SK<sub>c</sub>, and use it to decrypt chain-validators' message,
that would equip them with the symmetric key K<sub>I</sub> the invocation transaction's payload was encrypted with.
Given the latter, validators decrypt code-info, and invoke the chain-code function with the specified arguments, and the code-metadata attached(See Section X an d Y).
While the chain-code is executed, updates of the state of that chain-code are possible.
These are encrypted using the state-specific key K<sub>s</sub> that was defined during that chain-code's deployment.
In particular, K<sub>s</sub> is used the same way K<sub>iTx</sub> is used in the design of our first release.  

### 4.7 Invocation Access Control offered at the infrastructure
Invocation access control is in place to restrict access to invoking a chain-code function to a
subset of users of the system, as defined by the chain-code developer. We aim to offer this functionality in the future as part of our fabric.

The following figure reflects this capability of specifying invocation ACLs in the deployment of a chain-code.

![FirstRelease-deploy](./images/sec-futrel-depl.png)

More specifically,
the headers part of code-info contain the list of function prototypes exposed to the contract users each
followed by their invocation access control list (ACL).

Function ACLs are defined by the means of certificates (enrollment or transaction) and describe the set of users who should be able to invoke that function. These ACLs, are taken in consideration
when a function of that chain-code is invoked, i.e., during the processing of the corresponding invocation transaction.
Headers are implicitly included in the payload (through their hash), but are encrypted using a different key from the code-payload.
In this way access to the headers can be given independently of access to the code itself, and users who are only allowed to invoke
a function, are able to do so without locally keeping state or being able to access the code.
Notice that (T)Certs of potential invokers (contract users) are listed in a plaintext form in the transaction to facilitate
efficient search over a user's trasactions.

The figure below demonstrates how the invoker authorizes itself to invoke a function.

![FirstRelease-deploy](./images/sec-futrel-inv.png)

It uses the (transaction) certificate of its that is present in the ACL of the invoked function in the associated deployment transaction
and signs the plaintext payload of its invocation with this certificate.
The invocation transaction is signed externally potentially with another (transaction) certificate of the invoker (depending on the configuration of the invoker's client). Notice that the signature on the
cleartext plaintext of the invocation transaction is linked to the certificate used to extenrally sign the transaction, as the signed message contains a hash of the creator's transaction certificate.
This is important, as it guarantees that copying the code-info part of the transaction to another user's transaction, and messing up with the system is not possible.

Validators who receive this transaction evaluate the payload plaintext signature with the (T)Certs that appear in the ACLs of the deployment transaction of the invoked chain-code,
and validate it. Based on whether the signature verification goes through the validators proceed with the execution of the transaction or output an error message.

### 4.8 Replay Attack Resistance
In replay attacks the attacker simply "replays" a message he "eavesdropped" on the network or ''saw'' on the Blockchain. Replay attacks are a big problem here, as they can incur into the validating entities re-doing a computationally intensive process (contract invocation) and/or affect the state of the corresponding contract, while it requires minimar or no power from the attacker side.  To make matters worse, if a transaction was a payment transaction, replays could potentially incur into the payment being performed more than once, without this being the intention of the payer. Existing systems resist replay attacks as follows:
* Record hashes of transactions in the system. This solution would require that validators maintain a log of the hash of each transaction that has ever been announced through the network, and compare a new transaction against their locally stored transaction record. Clearly such approach cannot scale for large networks, and could easily result into validators spending a lot of time to do the check of whether a transaction has been replayed, than executing the actual transaction.
* Leverage state that is maintained per user identity (Ethereum). Ethereum keeps some state, e.g., counter (initially set to 1) for each identity/pseudonym in the system. Users also maintain their own counter (initially set to 0) for each identity/pseudonym of theirs. Each time a user sends a transaction using an identity/pseudonym of his, he increases his local counter by one and adds the resulting value to the transaction. The transaction is subsequently signed by that user identity and released to the network. When picking up this transaction, validators check the counter value included within and compare it with the one they have stored locally; if the value is the same, they increase the local value of that identity's counter and accept the transaction. Otherwise, they reject the transaction as invalid or replay.  Although this would work well in cases where we have limited number of user identities/pseudonyms (e.g., not too large), it would ultimately not scale in a system where users use a different identifier (transaction certificate) per transaction, and thus have a number of user pseudonyms proportional to the number of transactions.

Other asset management systems, e.g., Bitcoin, though not directly dealing with replay attacks, they resist them. In systems that manage (digital) assets, state is maintained on a per asset basis, i.e., validators only keep a record of who owns what. Resistance to replay attacks come as a direct result from this, as replays of transactions would be immediately be deemed as invalid by the protocol (since can only be shown to be derived from older owners of an asset/coin). While this would be appropriate for asset management systems, this does not fot the needs of a Blockchain systems with more generic use than asset management.


Detailed description of the design of replay attack protection. For replay attack protection we adopt a hybrid approach. In general, we require that users add in the transaction a nonce that is generated in a different manner depending on whether the transaction is anonymous (followed and signed by a transaction certificate) or not (followed and signed by a long term enrollment certificate). In particular:
* Users submitting a transaction with their enrollment certificate should include in that transaction a nonce that is a function of the nonce they used in the previous transaction they issued with the same certificate (e.g., a counter function or a hash). The nonce included in the first transaction of each enrollment certificate can be either pre-fixed by the system (e.g., included in the genesis block) or chosen by the user. In the first case, the genesis block would need to include nonceall , i.e., a fixed number and the nonce used by user with identity IDA for his first enrollment certificate signed transaction would be
  nonce_round0IDAhash(IDA, nonce<sub>all</sub>),
  where IDA appears in the enrollment certificate. From that point onward successive transactions of
  that user with enrollment certificate would include a nonce as follows
  nonce<sub>round<sub>i</sub>IDA</sub> <- hash(nonce<sub>round<sub>{i-1}</sub>IDA</sub>),
  that is the nonce of the ith transaction would be using the hash of the nonce used in the {i-1}th transaction of that certificate. Validators here continue to process a transaction they receive parse, as long as it satisfies the condition mentioned above. Upon successful validation of transaction's format, the validators update their database with that nonce.

  **Storage overhead**:
  1. on the user side: only the most recently used nonce,
  2. on validator side: O(n), where n is the number of users.
* Users submitting a transaction with a transaction certificate
  should include in the transaction a random nonce, that would guarantee that
  two transactions do not result into the same hash. Validators add the hash of
  this transaction in their local database if the transaction certificate used within
  it has not expired. To avoid storing large amounts of hashes, we introduce validity periods
  in the transaction certificates, and require that we maintain an updated record of received
  transactions' hashes within the current or future validity period.

  **Storage overhead** (only makes sense for validators here):  O(m), where m is the approximate number of
  transactions within a validity period and corresponding validity period identifier (see below).

### 4.9 Access control on the application level

An application, is a piece of software that runs on top of a Blockchain client software, and,
performs a special task over the Blockchain, i.e., restaurant table reservation.
Application software have a version of
developer, enabling the latter to generate and manage a couple of chain-codes that are necessary for
the business this application serves, and a client-version that would allow the application's end-users
to make use of the application, by invoking these chain-codes.
The use of the Blockchain can be transparent to the application end-users or not.

In this Section we show how an application leveraging chain-codes can implement its own accecss control,
and guidelines on how our Membership services PKI can be leveraged for the same purpose.

We divide our presentation into enforcement of invocation access control,
and enforcement of read-access control by the application.


### 4.9.1 Invocation access control
To allow the application to implement its own invocation access control at the application layer securely, special support by the fabric must be provided.
In the following we elaborate on the tools exposed by the fabric to the application for this purpose, and provide guidelines on how these should be used by the application for the latter to enforce access control securely.


### 4.9.2 Support from the fabric layer
For *u<sub>c</sub>* to be able to implement its own invocation access control at the application layer securely, special support by the fabric must be provided. More specifically
fabric layer gives access to following capabilities:

1. The client-application can request the fabric to sign and verify any message with specific transaction certificates or enrollment certificate the client owns; this is expressed via the Certificate Handler interface
2. The client-application can request the fabric a unique *binding* to be used to bind authentication data of the application to the underlying transaction transporting it; this is expressed via the Transaction Handler interface,
//2. The fabric allows to *name* each transaction by means of a unique *binding* to be used to bind application data to the underlying transaction transporting it,
3. Support for a transaction format, that allows for the application to specify some metadata, that are passed to the chain-code at deployment, and invocation time; the latter denoted by code-metadata.


The **Certificate Handler** interface allows to sign and verify any message using signing key-pair underlying the associated certificate.
The certificate can be a TCert or an ECert.

```
// CertificateHandler exposes methods to deal with an ECert/TCert
type CertificateHandler interface {

    // GetCertificate returns the certificate's DER
    GetCertificate() []byte

    // Sign signs msg using the signing key corresponding to the certificate
    Sign(msg []byte) ([]byte, error)

    // Verify verifies msg using the verifying key corresponding to the certificate
    Verify(signature []byte, msg []byte) error

    // GetTransactionHandler returns a new transaction handler relative to this certificate
    GetTransactionHandler() (TransactionHandler, error)
}
```


The **Transaction Handler** interface allows to create transactions and give access to the underlying *binding* that can be leveraged to link
application data to the underlying transaction. Bindings are a concept that have been introduced in network transport protocols (See, https://tools.ietf.org/html/rfc5056),
known as *channel bindings*, that *allows applications to establish that the two end-points of a secure channel at one network layer are the same as at a higher layer
by binding authentication at the higher layer to the channel at the lower layer.
This allows applications to delegate session protection to lower layers, which has various performance benefits.*
Open Blockchain transaction bindings offer the ability to uniquely identify the fabric layer of the transaction that serves as the container that
application data uses to be added to the ledger.

```
// TransactionHandler represents a single transaction that can be uniquely determined or identified by the output of the GetBinding method.
// This transaction is linked to a single Certificate (TCert or ECert).
type TransactionHandler interface {

    // GetCertificateHandler returns the certificate handler relative to the certificate mapped to this transaction
    GetCertificateHandler() (CertificateHandler, error)

    // GetBinding returns a binding to the underlying transaction (container)
    GetBinding() ([]byte, error)

    // NewChaincodeDeployTransaction is used to deploy chaincode
    NewChaincodeDeployTransaction(chaincodeDeploymentSpec *obc.ChaincodeDeploymentSpec, uuid string) (*obc.Transaction, error)

    // NewChaincodeExecute is used to execute chaincode's functions
    NewChaincodeExecute(chaincodeInvocation *obc.ChaincodeInvocationSpec, uuid string) (*obc.Transaction, error)

    // NewChaincodeQuery is used to query chaincode's functions
    NewChaincodeQuery(chaincodeInvocation *obc.ChaincodeInvocationSpec, uuid string) (*obc.Transaction, error)
}
```
For version 1, *binding* consists of the *hash*(TCert, Nonce), where TCert, is the transaction certificate
used to sign the entire transaction, while Nonce, is the nonce number used within.

The **Client** interface is more generic, and offers a mean to get instances of the previous interfaces.

```
type Client interface {

    ...

    // GetEnrollmentCertHandler returns a CertificateHandler whose certificate is the enrollment certificate
    GetEnrollmentCertificateHandler() (CertificateHandler, error)

    // GetTCertHandlerNext returns a CertificateHandler whose certificate is the next available TCert
    GetTCertificateHandlerNext() (CertificateHandler, error)

    // GetTCertHandlerFromDER returns a CertificateHandler whose certificate is the one passed
    GetTCertificateHandlerFromDER(der []byte) (CertificateHandler, error)

}
```

To support application-level ACLs, the fabric's transaction and chaincode specification format have an additional field to store application-specific metadata.
This field is depicted in both figures 1, by code-metadata. The content of this field is decided by the application, at the transaction creation time.
The fabric layer treats it as an unstructured stream of bytes.



```

message ChaincodeSpec {

    ...

    ConfidentialityLevel confidentialityLevel;
    bytes metadata;

    ...
}


message Transaction {
    ...

    bytes payload;
    bytes metadata;

    ...
}
```

To assist chaincode execution, at the chain-code invocation time, the validators provide the chaincode with additional information, like the metadata and the binding.  



### 4.9.3 Implementing Invocation Access Control inside the Application
In this section, we elaborate on how the application can leverage the means provided by the fabric to implement its own access control on its chain-code functions.
For the purpose of our discussion we consider the following entities:

1. **C**: is a chaincode that contains a single function, e.g., called *hello*;
2. **u<sub>c</sub>**: is the **C** deployer;
3. **u<sub>i</sub>**: is a user who is authorized to invoke **C**'s functions.
User u<sub>c</sub> wants to ensure that only u<sub>i</sub> can invoke the function *hello*.

**Deployment of a Chain-code:** At deployment time, u<sub>c</sub> has full control on the deployment transaction's metadata,
 and can be used to store a list of ACLs (one per function), or a list of roles that are needed by the application. The format which is used to store these ACLs is up to the deployer's application, as the chain-code is the one
who would need to parse the metadata at execution time.
To define each of these lists/roles, u<sub>c</sub> can use any TCerts/Certs of the u<sub>i</sub> (or, if applicable, or other users who have been assigned that privilege or role). Let this be TCert<sub>u<sub>i</sub></sub>.
The exchange of TCerts or Certs among the developer and authorized users is done through an out-of-band channel.

Assume that the application of u<sub>c</sub>'s requires that to invoke the *hello* function, a certain message *M* has to be authenticated by an authorized invoker (u<sub>i</sub>, in our example).
We distinguish the following two cases:
1. *M* is one of the chaincode's function arguments;
2. *M* is the invocation message itself, i.e., function-name, function-arguments.

**Chain-code invocation: **
To invoke C, u<sub>i</sub>'s application needs to sign *M* using the TCert/ECert, that was used to identify u<sub>i</sub>'s participation in the chain-code at the associated
deployment transaction's metadata, i.e., TCert<sub>u<sub>i</sub></sub>. More specifically, u<sub>i</sub>'s client application does the following:

1. Retrieves a CertificateHandler for Cert<sub>u<sub>i</sub></sub>, *cHandler*;
2. obtains a new TransactionHandler to issue the execute transaction, *txHandler* relative to his next available TCert or his ECert;
3. gets *txHandler*'s *binding* by invoking *txHandler.getBinding()*;
4. signs *'*M* || txBinding'* by invoking *cHandler.Sign('*M* || txBinding')*, let *sigma* be the output of the signing function;
5. issues a new execute transaction by invoking, *txHandler.NewChaincodeExecute(...)*. Now, *sigma* can be included
  in the transaction as one of the arguments that are passed to the function (case 1) or as part of the code-metadata section of the payload(case 2).

**Chain-code processing:**
The validators, who receive the execute transaction issued u<sub>i</sub>, will provide to *hello* the following information:

1. The *binding* of the execute transaction, that can be independently computed at the validator side;
2. The *metadata* of the execute transaction (code-metadata section of the transaction);
3. The *metadata* of the deploy transaction (code-metadata component of the corresponding deployment transaction).

Notice that *sigma* is either part of the arguments of the invoked function, or stored inside the code-metadata of the invocation transaction (properly formatted by the client-application).
Application ACLs are included in the code-metadata section, that is also passed to the chain-code at execution time.
Function *hello* is responsible for checking that *sigma* is indeed a valid signature issued by TCert<sub>u<sub>i</sub></sub>, on *'*M* || txBinding'*.

### 4.9.4 Read Access Control Enforcement by the Application
In this section we show the way our Blockchain infrastructure offers support to the application to
enforce its own read-access control policies at the level of users. As in the case of invocation access
control, we first describe the infrastructure features that can be leveraged by the application for this
purpose, and then proceed on the way applications should use these tools.

For the purpose of this discussion, we leverage a similar example as before, i.e.,
1. **C**: is a chaincode that contains a single function, e.g., called *hello*;
2. **u<sub>A</sub>**: is the **C**'s deployer, also known as application;
3. **u<sub>r</sub>**: is a user who is authorized to read **C**'s functions.
User u<sub>A</sub> wants to ensure that only u<sub>r</sub> can read the function *hello*.

### 4.9.5 Support from the fabric layer.
For *u<sub>A</sub>* to be able to implement its own read access control at the application layer securely, our infrastructure is required to
support the transaction format for code deployment and invocation, as depicted in the two figures below.

![SecRelease-RACappDepl title="Deployment transaction format supporting application-level read access control."](./images/sec-RACapp-depl.png)

![SecRelease-RACappInv title="Invocation transaction format supporting application-level read access control."](./images/sec-RACapp-inv.png)

More specifically fabric layer is required to provide the following functionality:
1. Provide minimal encryption capability such that data is only decryptable by a validator's (infrastructure) side; this means that the
   infrastructure should move closer to our future version, where an asymmetric encryption scheme is used
   for encrypting transactions. More specifically, an asymmetric key-pair is used for the chain, denoted by K<sub>chain</sub> in the Figures above,
   but detailed in Section <a href="./txconf.md">Transaction Confidentiality</a>.
2. The client-application can request the infrastructure sitting on the client-side to encrypt/decrypt information using a specific public
   encryption key, or that client's long-term decryption key.
3. The transaction format offers the ability to the application to store additional transaction metadata, that can be passed to the
   client-application after the latter's request. Transaction metadata, as opposed to code-metadata, is not encrypted or provided to
   the chain-code at execution time. Validators treat these metadata as a list of bytes they are not responsible for checking validity of.

### 4.9.6 Enforcement of read-access control on the application.
For this reason the application may request and obtain access to the public encryption key of the user **u<sub>r</sub>**; let that be **PK<sub>u<sub>r</sub></sub>**. Optionally,
**u<sub>r</sub>** may be providing **u<sub>A</sub>** with a certificate of its, that would be leveraged by the application, say, TCert<sub>u<sub>r</sub></sub>; given the latter,
the application would, e.g., be able to trace that user's transactions w.r.t. the application's chain-codes. TCert<sub>u<sub>r</sub></sub>, and PK<sub>u<sub>r</sub></sub>, are
exchanged in an out-of-band channel.

At deployment time, application **u<sub>A</sub>** performs the following steps:
1. Uses the underlying infrastructure to encrypt the information of **C**, the application would like to make accessible to **u<sub>r</sub>**, using PK<sub>u<sub>r</sub></sub>.
   Let C<sub>u<sub>r</sub></sub> be the resulting ciphertext.
2. (optioanal) C<sub>u<sub>r</sub></sub> is concatenated with can be (optionally) concatenated with TCert<sub>u<sub>r</sub></sub>
3. Pass the overall string as ''Tx-metadata'' of the confidential transaction to be constructed.

At invocation time, the client-application on u<sub>r</sub>'s node, would be able, by obtaining the deployment transaction to retrieve the content of **C**.
It just needs to retrieve the **tx-metadata** field of the associated deployment transaction, and trigger the decryption functionality offered by our Blockchain
infrastrucure's client, for C<sub>u<sub>r</sub></sub>. Notice that it is the application's responsibility to encrypt the correct **C** for u<sub>r</sub>.
Also, the use of **tx-metadata** field can be generalized to accommodate application-needs. E.g., it can be that invokers leverage the same field of invocation transactions
to pass information to the developer of the application, etc.

<font color="red">**Important Note:** </font> It is essential to note that validators **do not provide** any decryption oracle to the chain-code throughout its execution. Its infrastructure is though
responsible for decrypting the payload of the chain-code itself (as well as the code-metadata fields near it), and provide those to containers for
deployment/execution.

### 4.10 Network security (TLS)
The TLS CA should be capable of issuing TLS certificates to (non-validating) peers, validators, and individual clients (or browsers capable of storing a private key). Preferably, these certificates are distinguished by type, per above. TLS certificates for CAs of the various types (such as TLS CA, ECA, TCA) could be issued by an intermediate CA (i.e., a CA that is subordinate to the root CA). Where there is not a particular traffic analysis issue, any given TLS connection can be mutually authenticated, except for requests to the TLS CA for TLS certificates.

In the current implementation the only trust anchor is the TLS CA self-signed certificate in order to accommodate the limitation of a single port to communicate with all three (co-located) servers, i.e., the TLS CA, the TCA and the ECA. Consequently, the TLS handshake is established with the TLS CA, which passes the resultant session keys to the co-located TCA and ECA. The trust in validity of the TCA and ECA self-signed certificates is therefore inherited from trust in the TLS CA. In an implementation that does not thus elevate the TLS CA above other CAs, the trust anchor should be replaced with a root CA under which the TLS CA and all other CAs are certified.

### 4.11 Restrictions in the current release
 - we offer a minimal set of confidentiality properties where a chain-code is accessible by any entity that is member of the system, i.e., validators and users who have registered to our membership services, and not accessible by any-one else. The latter include any party that has access to the storage area where the ledger is maintained, or other entities that are able to see the transactions that are announced in the validator network. The design of the first release is detailed in subsection 4.10.1
 - the code utilizes self-signed certificates for entities such as the enrollment CA (ECA) and the transaction CA (TCA)
 - invocation access control enforcement is not currently provided
 - Replay attack resistance mechanisms is not available
 - we only offer the means to the application to implement its own invocation access control: it is up to the application to leverage the infrastructure's tools properly for security to be guaranteed. This means, that if the application ignores to *bind* the transaction binding offered by our fabric, secure transaction processing  may be at risk.

### 4.11.1 Simplified Transaction Confidentiality

**Disclaimer:** We emphasize that the first release design on confidentiality is a minimal design for the purpose of the first release, and will be used as an intermediate step to reach a design that allows for fine grain (invocation) access control enforcement in the next versions.



For the first release we offer confidentiality of transactions at the chain-level, i.e., we require that the content of a transaction included in a ledger, is readable by all members of that chain, i.e., validators and users. At the same time, we offer fine-graim auditing capabilities, such that an (application) auditor can be given the means to perform auditing by passively observing the Blockchain data, and while guaranteeing that he is given access solely to the transactions related to the application under audit. State is encrypted in a way such that our auditing requirement is satisfied, while not disrupting the proper operation of the underlying consensus network.



More specifically, for this release we stay simple and efficient, and employ only symmetric key encryption. In this setting, one of the main threats is represented by cryptanalysis[3,4] that leverage the availability of many ciphertexts being encrypted under the same key. Another important challenge that is specific to the blockchain setting, is that validators need to run consensus over the state of the blockchain, that, aside the transactions themselves, also includes the state updates of individual contracts or chaincodes. Though this is trivial to do for non-confidential chaincodes,  for confidential chaincodes, one needs to design the state encryption mechanism such that the resulting ciphertexts are semantically secure, and yet, identical if the plaintext state is the same.



To overcome both challenges, we design a key hierarchy that reduces the number of ciphertexts exposed, that are encrypted under the same key. At the same time, as we use some of these keys for the generation of IVs, this allows the validating parties to generate exactly the same ciphertext when executing the same transaction (this is necessary to remain agnostic to the underlying consensus algorithm) and offers the possibility of fine-grained auditing by disclosing to auditing entities only the most relevant keys.



Method description: We assume that (though we do not deal with the details of how this can be achieved) when Open Blockchain is first deployed, our membership service generates a symmetric key for the ledger (K<sub>chain</sub>) that is distributed

at registration time to all the entities of the blockchain system, i.e., the clients and the validating entities that have issued credentials through the membership service of the chain. At enrollment phase, user obtain (as before) an enrollment certificate, denoted by Cert<sub>u<sub>i</sub></sub> for user u<sub>i</sub> , while each validator v<sub>j</sub> obtain its enrollment certificate denoted by Cert<sub>v<sub>j</sub></sub>.

Entity enrollment would be enhanced, as follows. In addition to enrollment certificates, users who wish to anonymously participate in transactions issue transaction certificates. For simplicity transaction certificates of a user u<sub>i</sub> are denoted by TCert<sub>u<sub>i</sub></sub>. Transaction certificates include the public part of a signature key-pair denoted by (tpk<sub>u<sub>i</sub></sub>,tsk<sub>u<sub>i</sub></sub>).



In order to defeat crypto-analysis and enforce confidentiality, we envision the following key hierarchy, and generation and validation of confidential transactions:



To submit a confidential transaction (Tx) to the ledger, a client first samples a nonce (N), which is required to be unique among all the transactions submitted to the blockchain, and derive a transaction symmetric

key (K<sub>Tx</sub>) by applying the HMAC function keyed with K<sub>chain</sub> and on input the nonce, K<sub>Tx</sub>= HMAC(K<sub>chain</sub>, N). From K<sub>Tx</sub>, the client derives two AES keys:

K<sub>TxCID</sub> as HMAC(K<sub>Tx</sub>, c<sub>1</sub>), K<sub>TxP</sub> as HMAC(K<sub>Tx</sub>, c<sub>2</sub>)) to encrypt respectively the chain-code name or identifier CID and code (or payload) P.

c<sub>1</sub>, c<sub>2</sub> are public constants. The nonce, the Encrypted Chaincode ID (ECID) and the Encrypted Payload (EP) are added in the transaction Tx structure, that is finally signed and so

authenticated. Figure below shows how encryption keys for the client's transaction are generated. Arrows in this figure denote application of an HMAC, keyed by the key at the source of the arrow and

using the number in the arrow as argument. Deployment/Invocation transactions' keys are indicated by d/i respectively.



![FirstRelease-clientSide](./images/sec-firstrel-1.png)



To validate a confidential transaction Tx submitted to the blockchain by a client,

a validating entity first decrypts ECID and EP by re-deriving K<sub>TxCID</sub> and K<sub>TxP</sub>

from K<sub>chain</sub> and Tx.Nonce as done before. Once the Chaincode ID and the

Payload are recovered the transaction can be processed.







![FirstRelease-validatorSide](./images/sec-firstrel-2.png)



When V validates a confidential transaction, the corresponding chaincode can access and modify the

chaincode's state. V keeps the chaincode\'s state encrypted. In order to do so, V generates symmetric

keys as depicted in the figure above. Let iTx be a confidential transaction invoking a function

deployed at an early stage by the confidential transaction dTx (notice that iTx can be dTx itself

in the case, for example, that dTx has a setup function that initializes the chaincode's state).

Then, V generates two symmetric keys  K<sub>IV</sub>  and K<sub>state</sub> as follows:

1. It computes  as  K<sub>dTx</sub> , i.e., the transaction key of the corresponding deployment

transaction, and then N<sub>state</sub> = HMAC(K<sub>dtx</sub> ,hash(N<sub>i</sub>)), where N<sub>i</sub>

is the nonce appearing in the invocation transaction, and *hash* a hash function.

2. It sets K<sub>state</sub> = HMAC(K<sub>dTx</sub>, c<sub>3</sub> || N<sub>state</sub>),

truncated opportunely deeding on the underlying cipher used to encrypt; c<sub>3</sub> is a constant number

3. It sets K<sub>IV</sub> = HMAC(K<sub>dTx</sub>, c<sub>4</sub> || N<sub>state</sub>); c<sub>4</sub> is a constant number





In order to encrypt a state variable S, a validator first generates the IV as HMAC(K<sub>IV</sub>, crt<sub>state</sub>)

properly truncated, where crt<sub>state</sub> is a counter value that increases each time a state update

is requested for the same chaincode invocation. The counter is discarded after the execution of

the chaincode terminates. After IV has been generated, V encrypts with authentication (i.e., GSM mode)

the value of S concatenated with Nstate(Actually, N<sub>state</sub>  doesn't need to be encrypted but

only authenticated). To the resulting ciphertext (CT), N<sub>state</sub> and the IV used is appended.

In order to decrypt an encrypted state CT|| N<sub>state'</sub> , a validator first generates the symmetric

keys K<sub>dTX</sub>' ,K<sub>state</sub>' using N<sub>state'</sub>  and as and then decrypts CT.



Generation of IVs: In order to be agnostic to any underlying consensus algorithm, all the validating

parties need a method to produce the same exact ciphertexts. In order to do use, the validators need

to use the same IVs. Reusing the same IV with the same symmetric key completely breaks the security

of the underlying cipher. Therefore, the process described before is followed. In particular, V first

derives an IV generation key K<sub>IV</sub> by computing HMAC(K<sub>dTX</sub>, c<sub>4</sub> || N<sub>state</sub> ),

where c<sub>4</sub> is a constant number, and keeps a counter crt<sub>state</sub> for the pair

(dTx, iTx) with is initially set to 0. Then, each time a new ciphertext has to be generated, the validator

generates a new IV by computing it as the output of HMAC(K<sub>IV</sub>, crt<sub>state</sub>)

and then increments the crt<sub>state</sub> by one.



Another benefit that comes with the above key hierarchy is the ability to allow fine-grained auditing.

For example, by releasing K<sub>chain</sub> we give access to the whole chain, by releasing only K<sub>state</sub>

for a given pair of transactions (dTx,iTx) we give access to state updated by iTx, and so on.



Figure 3 demonstrates the format of a deployment transaction for the first release.



![FirstRelease-deploy](./images/sec-firstrel-depl.png)





Figure 4 demonstrates the format of an invocation transaction for the first release.



![FirstRelease-deploy](./images/sec-firstrel-inv.png)





One can notice that both deployment and invocation transactions consist of two sections:

* section general-info, that contains the administration details of the transaction, i.e., which chain this transaction corresponds to (chained), the type of transaction (that is set to ''deploymTx'' or ''invocTx'', the version number of confidentiality policy implemented, its creator identifier (expressed by means of TCert of Cert), and a nonce, that facilitates primarily replay-attack resistance techniques.

* section code-info, that contains information on the chain-code source code. For deployment transaction this is essentially the chain-code identifier/name and source code, while for invocation chain-code is the name of the function invoked and its arguments. As shown in the two figures code-info in both transactions are encrypted ultimately using the chain-specific symmetric key Kchain.

## 5. Byzantine Consensus
The ``obcpbft`` package is an implementation of the seminal [PBFT](http://dl.acm.org/citation.cfm?id=571640 "PBFT") consensus protocol [1], which provides consensus among validators despite a threshold of validators acting as _Byzantine_, i.e., being malicious or failing in an unpredictable manner. In the default configuration, PBFT tolerates up to t<n/3 Byzantine validators.

Besides providing a reference implementation of the PBFT consensus protocol, ``obcpbft`` plugin contains also implementation of the novel _Sieve_ consensus protocol. Basically the idea behind Sieve is to provide a fabric-level protection from _non-deterministic_ transactions, which PBFT and similar existing protocols do not offer. ``obcpbft`` is easily configured to use either the classic PBFT or Sieve.  

In the default configuration, both PBFT and Sieve are designed to run on at least *3t+1* validators (replicas), tolerating up to *t* potentially faulty (including malicious, or *Byzantine*) replicas.

### 5.1 Overview
The `obcpbft` plugin provides a modular implementation of the `CPI` interface which can be configured to run PBFT or Sieve consensus protocol. The modularity comes from the fact that, internally, `obcpbft` defines the `innerCPI`  interface (i.e., the _inner consensus programming interface_), that currently resides in `pbft-core.go`.

The `innerCPI` interface defines all
interactions between the inner PBFT consensus (called here *core PBFT* and implemented in `pbft-core.go`) and the outer consensus that uses the core PBFT.  This outer consensus is called *consumer* within core PBFT. `obcpbft` package contains implementations of several core PBFT consumers:

  - `obc-classic.go`, a shim around core PBFT that implements the `innerCPI` interface and calls into the `CPI` interface;
  - `obc-batch.go`, an `obc-classic` variant that adds batching capabilities to PBFT; and  
  - `obc-sieve.go`, a core PBFT consumer that implements Sieve consensus protocol and `innerCPI` interface, calling into the `CPI interface`.

In short, besides calls to send messages to other peers (`innerCPI.broadcast` and `innerCPI.unicast`), the `innerCPI` interface defines indications that the core consensus protocol (core PBFT) exports to the consumer. These indications are modeled after a classical *total order (atomic) broadcast* API [2], with `innerCPI.execute` call being used to signal the atomic delivery of a message. Classical total order broadcast is augmented with *external validity* checks [2] (`innerCPI.verify`) and a functionality similar to the unreliable eventual leader failure detector &Omega; [3] (`innerCPI.viewChange`).

Besides `innerCPI`, core PBFT is defined by a set of calls into core PBFT. The most important call into core PBFT is `request` which is effectively used to invoke a total order broadcast primitive [2]. In the following, we first overview calls into core PBFT and then detail the ``innerCPI`` interface. Then, we briefly describe Sieve consensus protocol which will be specified and described in more details elsewhere.  

### 5.2 Core PBFT Functions
The following functions control for parallelism using a non-recursive lock and can therefore be invoked from multiple threads in parallel. However, the functions typically run to completion and may invoke functions from the CPI passed in.  Care must be taken to prevent livelocks.

#### 5.2.1 newPbftCore

Signature:

```
func newPbftCore(id uint64, config *viper.Viper, consumer innerCPI, ledger consensus.Ledger) *pbftCore
```

The `newPbftCore` constructor instantiates a new PBFT box instance, with the specified `id`.  The `config` argument defines operating parameters of the PBFT network: number replicas *N*, checkpoint period *K*, and the timeouts for request completion and view change duration.

| configuration key            | type       | example value | description                                                    |
|------------------------------|------------|---------------|----------------------------------------------------------------|
| `general.N`                  | *integer*  | 4             | Number of replicas                                             |
| `general.K`                  | *integer*  | 10            | Checkpoint period                                              |
| `general.timeout.request`    | *duration* | 2s            | Max delay between request reception and execution              |
| `general.timeout.viewchange` | *duration* | 2s            | Max delay between view-change start and next request execution |

The arguments `consumer` and `ledger` pass in interfaces that are used
to query the application state and invoke application requests once
they have been totally ordered.  See the respective sections below for
these interfaces.


#### 5.2.2 request

Signature:

```
func (pbft *pbftCore) request(msgPayload []byte) error
```

The `request` method takes an opaque request payload and introduces this request into the total order consensus.  This payload will be passed to the CPI `execute` function on all correct, up-to-date replicas once PBFT processing is complete.  The `request` method does not wait for execution before returning; `request` merely submits the request into the consensus.

PBFT does not support submission of the same request multiple times, i.e. a nonce is required if the same conceptual request has to be executed multiple times.  However, PBFT does not reliably prevent replay of requests; a nonce or sequence number can be used by the application to prevent against replays by a Byzantine client.

In rare cases, a `request` may be dropped by the network, and it will never `execute`; if the consumer cannot tolerate this, the consumer needs to implement retries itself.

#### 5.2.3 receive

Signature:

```
func (pbft *pbftCore) receive(msgPayload []byte) error
```

The `receive` method takes an opaque message payload, which another instance passed to the `broadcast` or `unicast` CPI functions.  All communication is expected to ensure integrity and provide authentication; e.g. by the use of TLS.  Note that currently authentication is not yet used.  Once authentication is provided, the function signature of `receive` should include the id of the sending node.

See also the discussion below regarding `innerCPI.broadcast` and `innerCPI.unicast`.


#### 5.2.4 close

Signature:

```
func (pbft *pbftCore) close()
```

The `close` method terminates all background operations.  This interface is mostly exposed for testing, because during operation of the obc peer, there is never a need to terminate the PBFT instance.

### 5.3 Inner Consensus Programming Interface

The consumer application provides the inner consensus programming interface to core PBFT.  PBFT will call these functions to query state and signal events.

Definition:

```
type innerCPI interface {
	broadcast(msgPayload []byte)
	unicast(msgPayload []byte, receiverID uint64) (err error)
	validate(txRaw []byte) error
	execute(txRaw []byte, rawMetadata []byte)
	viewChange(curView uint64)
}
```

#### 5.3.1 broadcast

Signature:

```
func (cpi innerCPI) broadcast(msgPayload []byte)
```

The `broadcast` function takes an opaque payload and delivers it to all other replicas via their `receive` method.  Messages may be lost or reordered.  See also the section on `receive` call coming into core PBFT.

#### 5.3.2 unicast

Signature:

```
func (cpi innerCPI) unicast(msgPayload []byte, receiverID uint64) (err error)
```

The `unicast` function is similar to `broadcast`, but takes a destination replica id.

#### 5.3.3 validate

Signature:

```
func (cpi innerCPI) validate(txRaw []byte) error
```
The `validate` function is invoked whenever PBFT receives a new request, either locally via `request`, or via consensus messages.  The argument of `validate` is the opaque request that was provided to the PBFT `request` method.  If `validate` returns a non-`nil` error, the local replica will discard the request and behave as if it had never received the request.

The `validate` function can be used for syntactic validation of application requests (i.e., *external validity* checks [2]).  Care must be taken not to introduce non-determinism when validating requests; i.e. the validation must not use any state, e.g., if different replicas receive `validate` calls in different sequence, also with respect to `execute`.  If non-determinism occurs during validation, the behavior of different replicas may diverge, which may lead to dropped requests or complete malfunction of the consensus.

#### 5.3.4 execute

Signature:

```
func (cpi innerCPI) execute(txRaw []byte, opts ...interface{})
```

PBFT will invoke the `execute` function when a request has been successfully totally ordered by the consensus protocol.  The argument passed to `execute` is the opaque request, as it has been previously passed to `request`.  All correct, up-to-date replicas will receive the same sequence of `execute` calls.  The application must be deterministic when processing the request.  Any non-determinism will lead to the state on replicas diverging, which is considered a byzantine behavior.

See also the discussion above on request replays in the `request` section.

#### 5.3.5 viewChange

Signature:

```
func (cpi innerCPI) viewChange(curView uint64)
```

The `viewChange` function is called by PBFT to signal a successful transition to a new view (and with it, a new primary).  This information is right now only of interest to the *Sieve* consensus algorithm, which uses PBFT leader election to avoid having to implement its own.

Assuming a fixed number of replicas, it is simple to map curView uint64 to replica ID using modulo arithmetic. Having this in mind, with core PBFT implementation, assuming eventual synchrony [4], it is straightforward to argue that the functionality of the `viewChange` call allows simple implementation of the *eventual leader* unreliable failure detector &Omega; [3].  

### 5.4 Sieve Consensus protocol

The design goal of Sieve is to augment PBFT consensus protocol with three main design goals:

- Enabling *consensus on the output state of replicas*, in addition to the consensus on the input state provided by PBFT. To achieve this, Sieve adopts the Execute-Verify (Eve) pattern introduced in [5].

- As OBC allows execution of arbitrary chaincode, such chaincode may introduce *non-deterministic* transactions. Although non-deterministic transaction should in principle be disallowed by, e.g., careful inspection of chaincode, using domain specific languages (DSLs), or by otherwise enforcing determinism, the design goal of Sieve is to provide a separate *consensus fabric-level* protection against *non-deterministic* transactions that can be used in combination with the above mentioned approaches.

	To this end, Sieve detects and *sieves out non-deterministic transactions* (that manifest themselves as such). Hence, Sieve does not require all input transactions to consensus (i.e., the replicated state machine) to be deterministic. This feature of Sieve is new and has not been implemented by any existing Byzantine fault tolerant consensus protocols.

A protocol achieving the above two goals should not be designed and implemented from scratch, and should reuse existing PBFT implementation, lowering code complexity and simplifying reasoning about a new consensus protocol. To this end, inspired by [6], Sieve is designed using a modular approach, reusing the core PBFT component of `obcpbft`.

Although the details of Sieve will appear elsewhere, we briefly outline some design and implementation aspects below.

In a nutshell, Sieve requires replicas to deterministically agree on the output of the execution of a request.  If the request was deterministic in the first place, all correct replicas will have obtained the same output, and they can agree on this very result. However, if a request happens to produce divergent outputs at correct replicas, Sieve may  detect this divergent condition, and the replicas will agree to discard the result of the request, thereby retaining determinism.

Notice that, as discussed further below, Sieve allows false negatives, i.e., execution of *non-deterministic* requests that execute with the same result at a sufficient number of replicas. However, Sieve allows no false positives and any discarded request is certainly non-deterministic.

The Sieve protocol uses core PBFT to agree on whether to accept or discard a request.  Execution of requests to Sieve is coordinated by a *leader*, which maps to the current PBFT primary (leveraging `innerCPI.viewchange` notification from core PBFT) .  Upon a new request, the leader will instruct all replicas to tentatively execute the request.  Every replica then reports the tentative result (i.e. application state) back to the leader.  The leader collects these *verify* reports in a *verify-set*, which unambiguously determines whether the request should be accepted or discarded.  This verify-set is then passed through the total order of core PBFT.

When core PBFT executes this verify-set, all correct replicas will act in the same way.  If the verify-set proves that execution diverged between correct replicas, the request is considered non-deterministic, and the replicas will roll back the tentative execution and restore the original application state.  If all correct replicas obtained the same result for the tentative execution, the replicas accept the execution and commit the tentative application state.

Under adverse conditions, a request that diverged between correct replicas may appear like a deterministic request (we speak of *false negative* in Sieve detection of non-determinstic requests).  Nevertheless, Sieve requires at least one correct replica to obtain a certain outcome state in order for that state to be committed. Correct replicas that possibly observe diverging execution will discard their result and synchronize their state to match the agreed-upon execution.


## 6. Application Programming Interface

The primary interface to OBC is a REST API. The REST API allows applications to register users, query the blockchain, and to issue transactions. A CLI is also provided to cover a subset of the available APIs for development purposes. The CLI enables developers to quickly test chaincodes or query for status of transactions.

Applications interact with a non-validating peer node through the REST API, which will require some form of authentication to ensure the entity has proper privileges. The application is responsible for supplying the appropriate authentication mechanism and the peer node will subsequently sign the outgoing messages with the client identity.

![Reference architecture](images/refarch-api.png) <p>
The OBC API design covers the categories below, though the implementation is incomplete for some of them in the current release. The [REST API](#62-rest-api) section will describe the APIs currently supported.

*  Identity - Enrollment to get certificates or revoking a certificate
*  Address - Target and source of a transaction
*  Transaction - Unit of execution on the ledger
*  Chaincode - Program running on the ledger
*  Blockchain - Contents of the ledger
*  Network - Information about the blockchain network
*  Storage - External store for files or documents
*  Event Stream - Sub/pub events on blockchain

## 6.1 REST Service
The Open Blockchain REST service can be enabled (via configuration) on either validating or non-validating peers, but it is recommended to only enabled the REST service on non-validating peers on production networks.

```
func StartOpenchainRESTServer(server *oc.ServerOpenchain, devops *oc.Devops)
```

This function reads the `rest.address` value in `obc-peer/openchain.yaml` configuration file, which is the configuration file for the `obc-peer` process. The value of the `rest.address` key defines the default address and port on which the peer will listen for HTTP REST requests.

It is assumed that the REST service receives requests from applications which have already authenticated the end user.

## 6.2 REST API

You can work with the OBC REST API through any tool of your choice. For example, the curl command line utility or a browser based client such as the Firefox Rest Client or Chrome Postman. You can likewise trigger REST requests directly through [Swagger](http://swagger.io/). To obtain the OBC REST API Swagger description, click [here](https://github.com/openblockchain/obc-peer/blob/master/openchain/rest/rest_api.json). The currently available APIs are summarized in the following section.

### 6.2.1 REST Endpoints

* [Block](#6211-block-api)
  * GET /chain/blocks/{block-id}
* [Chain](#6212-chain-api)
  * GET /chain
* [Transactions](#6213-transactions-api)
  * GET /transactions/{UUID}
* [Chaincode](#6214-devops-chaincode-api)
  * POST /chaincode
  * PUT /chaincode/{chaincode-id}
  * GET /chaincode/{chaincode-id}
  * DELETE /chaincode/{chaincode-id}
  * POST /chaincode/{chaincode-id}
* [Registrar](#6215-registrar-member-services)
  * POST /registrar
  * GET /registrar/{enrollmentID}
  * DELETE /registrar/{enrollmentID}
  * GET /registrar/{enrollmentID}/ecert

#### 6.2.1.1 Block API

* **GET /chain/blocks/{block-id}**

Use the Block API to retrieve the contents of various blocks from the blockchain. The returned Block message structure is defined in section [3.2.1.1](#3211-block).

Block Retrieval Request:
```
GET host:port/chain/blocks/173
```

Block Retrieval Response:
```
{
    "transactions": [
        {
            "type": 3,
            "chaincodeID": "EgRteWNj",
            "payload": "Ch4IARIGEgRteWNjGhIKBmludm9rZRIBYRIBYhICMTA=",
            "uuid": "f5978e82-6d8c-47d1-adec-f18b794f570e",
            "timestamp": {
                "seconds": 1453758316,
                "nanos": 206716775
            },
            "cert": "MIIB/zCCAYWgAwIBAgIBATAKBggqhkjOPQQDAzApMQswCQYDVQQGEwJVUzEMMAoGA1UEChMDSUJNMQwwCgYDVQQDEwN0Y2EwHhcNMTYwMTI1MjE0MTE3WhcNMTYwNDI0MjE0MTE3WjArMQswCQYDVQQGEwJVUzEMMAoGA1UEChMDSUJNMQ4wDAYDVQQDEwVsdWthczB2MBAGByqGSM49AgEGBSuBBAAiA2IABC/BBkt8izf6Ew8UDd62EdWFikJhyCPY5VO9Wxq9JVzt3D6nubx2jO5JdfWt49q8V1Aythia50MZEDpmKhtM6z7LHOU1RxuxdjcYDOvkNJo6pX144U4N1J8/D3A+97qZpKN/MH0wDgYDVR0PAQH/BAQDAgeAMAwGA1UdEwEB/wQCMAAwDQYDVR0OBAYEBAECAwQwDwYDVR0jBAgwBoAEAQIDBDA9BgYqAwQFBgcBAf8EMABNbPHZ0e/2EToi0H8mkouuUDwurgBYuUB+vZfeMewBre3wXG0irzMtfwHlfECRDDAKBggqhkjOPQQDAwNoADBlAjAoote5zYFv91lHzpbEwTfJL/+r+CG7oMVFUFuoSlvBSCObK2bDIbNkW4VQ+ZC9GTsCMQC5GCgy2oZdHw/x7XYzG2BiqmRkLRTiCS7vYCVJXLivU65P984HopxW0cEqeFM9co0=",
            "signature": "MGUCMCIJaCT3YRsjXt4TzwfmD9hg9pxYnV13kWgf7e1hAW5Nar//05kFtpVlq83X+YtcmAIxAK0IQlCgS6nqQzZEGCLd9r7cg1AkQOT/RgoWB8zcaVjh3bCmgYHsoPAPgMsi3TJktg=="
        }
    ],
    "stateHash": "7ftCvPeHIpsvSavxUoZM0u7o67MPU81ImOJIO7ZdMoH2mjnAaAAafYy9MIH3HjrWM1/Zla/Q6LsLzIjuYdYdlQ==",
    "previousBlockHash": "lT0InRg4Cvk4cKykWpCRKWDZ9YNYMzuHdUzsaeTeAcH3HdfriLEcTuxrFJ76W4jrWVvTBdI1etxuIV9AO6UF4Q==",
    "nonHashData": {
        "localLedgerCommitTimestamp": {
            "seconds": 1453758316,
            "nanos": 250834782
        }
    }
}
```

#### 6.2.1.2 Chain API

* **GET /chain**

Use the Chain API to retrieve the current state of the blockchain. The returned BlockchainInfo message is defined below.

```
message BlockchainInfo {
    uint64 height = 1;
    bytes currentBlockHash = 2;
    bytes previousBlockHash = 3;
}
```

* `height` - Number of blocks in the blockchain, including the genesis block.

* `currentBlockHash` - The hash of the current or last block.

* `previousBlockHash` - The hash of the previous block.

Blockchain Retrieval Request:
```
GET host:port/chain
```

Blockchain Retrieval Response:
```
{
    "height": 174,
    "currentBlockHash": "lIfbDax2NZMU3rG3cDR11OGicPLp1yebIkia33Zte9AnfqvffK6tsHRyKwsw0hZFZkCGIa9wHVkOGyFTcFxM5w==",
    "previousBlockHash": "Vlz6Dv5OSy0OZpJvijrU1cmY2cNS5Ar3xX5DxAi/seaHHRPdssrljDeppDLzGx6ZVyayt8Ru6jO+E68IwMrXLQ=="
}
```

#### 6.2.1.3 Transactions API

* **GET /transactions/{UUID}**

Use the Transaction API to retrieve an individual transaction matching the UUID from the blockchain. The returned transaction message is defined in section [3.1.2.1](#3121-transaction-data-structure).

Transaction Retrieval Request:
```
GET host:port/transactions/f5978e82-6d8c-47d1-adec-f18b794f570e
```

Transaction Retrieval Response:
```
{
    "type": 3,
    "chaincodeID": "EgRteWNj",
    "payload": "Ch4IARIGEgRteWNjGhIKBmludm9rZRIBYRIBYhICMTA=",
    "uuid": "f5978e82-6d8c-47d1-adec-f18b794f570e",
    "timestamp": {
        "seconds": 1453758316,
        "nanos": 206716775
    },
    "cert": "MIIB/zCCAYWgAwIBAgIBATAKBggqhkjOPQQDAzApMQswCQYDVQQGEwJVUzEMMAoGA1UEChMDSUJNMQwwCgYDVQQDEwN0Y2EwHhcNMTYwMTI1MjE0MTE3WhcNMTYwNDI0MjE0MTE3WjArMQswCQYDVQQGEwJVUzEMMAoGA1UEChMDSUJNMQ4wDAYDVQQDEwVsdWthczB2MBAGByqGSM49AgEGBSuBBAAiA2IABC/BBkt8izf6Ew8UDd62EdWFikJhyCPY5VO9Wxq9JVzt3D6nubx2jO5JdfWt49q8V1Aythia50MZEDpmKhtM6z7LHOU1RxuxdjcYDOvkNJo6pX144U4N1J8/D3A+97qZpKN/MH0wDgYDVR0PAQH/BAQDAgeAMAwGA1UdEwEB/wQCMAAwDQYDVR0OBAYEBAECAwQwDwYDVR0jBAgwBoAEAQIDBDA9BgYqAwQFBgcBAf8EMABNbPHZ0e/2EToi0H8mkouuUDwurgBYuUB+vZfeMewBre3wXG0irzMtfwHlfECRDDAKBggqhkjOPQQDAwNoADBlAjAoote5zYFv91lHzpbEwTfJL/+r+CG7oMVFUFuoSlvBSCObK2bDIbNkW4VQ+ZC9GTsCMQC5GCgy2oZdHw/x7XYzG2BiqmRkLRTiCS7vYCVJXLivU65P984HopxW0cEqeFM9co0=",
    "signature": "MGUCMCIJaCT3YRsjXt4TzwfmD9hg9pxYnV13kWgf7e1hAW5Nar//05kFtpVlq83X+YtcmAIxAK0IQlCgS6nqQzZEGCLd9r7cg1AkQOT/RgoWB8zcaVjh3bCmgYHsoPAPgMsi3TJktg=="
}
```

#### 6.2.1.4 Chaincode API

* **POST /chaincode**
* **PUT /chaincode/{chaincode-id}**
* **GET /chaincode/{chaincode-id}**

Use the Chaincode APIs to deploy, invoke, and query chaincodes. The deploy request requires the client to supply a `path` parameter, pointing to the location of the chaincode in the file system. The response to a deploy request is either a message containing a confirmation of successful chaincode deployment or an error, containing a reason for the failure. It also contains the generated chaincode `name` in the `message` field, which is to be used in subsequent invocation and query transactions to identify the deployed chaincode.

To deploy a chaincode, supply the required ChaincodeSpec payload, defined in section [3.1.2.2](#3122-transaction-specification).

Deploy Request:
```
POST host:port/chaincode

{
  "type": "GOLANG",
  "chaincodeID":{
      "path":"github.com/openblockchain/obc-peer/openchain/example/chaincode/chaincode_example02"
  },
  "ctorMsg": {
      "function":"init",
      "args":["a", "100", "b", "200"]
  }
}

```

Deploy Response:
```
{
    "OK": "Successfully deployed chainCode.",
    "message": "3940678a8dff854c5ca4365fe0e29771edccb16b2103578c9d9207fea56b10559b43ff5c3025e68917f5a959f2a121d6b19da573016401d9a028b4211e10b20a"
}
```

With security enabled, modify the required payload to include the `secureContext` element passing the enrollment ID of a logged in user as follows:

Deploy Request with security enabled:
```
{
  "type": "GOLANG",
  "chaincodeID":{
      "path":"github.com/openblockchain/obc-peer/openchain/example/chaincode/chaincode_example02"
  },
  "ctorMsg": {
      "function":"init",
      "args":["a", "100", "b", "200"]
  },
  "secureContext": "lukas"
}
```

The invoke request requires the client to supply a `name` parameter, which was previously returned in the response from the deploy transaction. The response to an invocation request is either a message containing a confirmation of successful execution or an error, containing a reason for the failure.

To invoke a function within a chaincode, supply the required ChaincodeInvocationSpec defined in section [3.1.2.4](#3124-invoke-transaction).

Invoke Request:
```
PUT host:port/chaincode/{chaincode-id}

{
  "chaincodeSpec": {
  	"type": "GOLANG",
  	"ctorMsg": {
    	"function":"invoke",
      	"args":["a", "b", "10"]
  	}
  }
}
```

Invoke Response:
```
{
    "OK": "Successfully invoked chainCode."
}
```

With security enabled, modify the required payload to include the `secureContext` element passing the enrollment ID of a logged in user as follows:

Invoke Request with security enabled:
```
{
  "chaincodeSpec": {
  	"type": "GOLANG",
  	"ctorMsg": {
    	"function":"invoke",
      	"args":["a", "b", "10"]
  	},
  	"secureContext": "lukas"
  }
}
```

The query request requires the client to supply a `name` parameter, which was previously returned in the response from the deploy transaction. The response to a query request depends on the chaincode implementation. It will contain a message containing a confirmation of successful execution or an error, containing a reason for the failure. In the case of successful execution, the response will also contain values of requested state variables within the chaincode.

To invoke a query function within a chaincode, supply the required ChaincodeInvocationSpec defined in section [3.1.2.4](#3124-invoke-transaction).

Query Request:
```
GET host:port/chaincode/{chaincode-id}

{
  "chaincodeSpec": {
  	"type": "GOLANG",
  	"ctorMsg": {
    	"function":"query",
      	"args":["a"]
  	}
  }
}
```

Query Response:
```
{
    "OK": "80"
}
```

With security enabled, modify the required payload to include the `secureContext` element passing the enrollment ID of a logged in user as follows:

Query Request with security enabled:
```
{
  "chaincodeSpec": {
  	"type": "GOLANG",
  	"chaincodeID":{
    	"name":"3940678a8dff854c5ca4365fe0e29771edccb16b2103578c9d9207fea56b10559b43ff5c3025e68917f5a959f2a121d6b19da573016401d9a028b4211e10b20a"
  	},
  	"ctorMsg": {
    	"function":"query",
      	"args":["a"]
  	},
  	"secureContext": "lukas"
  }
}
```

#### 6.2.1.5 Registrar (member services)

* **POST /registrar**
* **GET /registrar/{enrollmentID}**
* **DELETE /registrar/{enrollmentID}**
* **GET /registrar/{enrollmentID}/ecert**

Use the Registrar APIs to manage end user registration with the certificate authority (CA). These API endpoints are used to register a user with the CA, determine whether a given user is registered, and to remove any login tokens for a target user from local storage, preventing them from executing any further transactions. The Registrar APIs are also used to retrieve user enrollment certificates from the system.

The `/registrar` endpoint is used to register a user with the CA. The required Secret payload is defined below. The response to the registration request is either a confirmation of successful registration or an error, containing a reason for the failure.

```
message Secret {
    string enrollId = 1;
    string enrollSecret = 2;
}
```

* `enrollId` - Enrollment ID with the certificate authority.

* `enrollSecret` - Enrollment password with the certificate authority.

Enrollment Request:
```
POST host:port/registrar

{
  "enrollId": "lukas",
  "enrollSecret": "NPKYL39uKbkj"
}
```

Enrollment Response:
```
{
    "OK": "Login successful for user 'lukas'."
}
```

The `GET /registrar/{enrollmentID}` endpoint is used to confirm whether a given user is registered with the CA. If so, a confirmation will be returned. Otherwise, an authorization error will result.

Verify Enrollment Request:
```
GET host:port/registrar/jim
```

Verify Enrollment Response:
```
{
    "OK": "User jim is already logged in."
}
```

Verify Enrollment Request:
```
GET host:port/registrar/alex
```

Verify Enrollment Response:
```
{
    "Error": "User alex must log in."
}
```

The `DELETE /registrar/{enrollmentID}` endpoint is used to delete login tokens for a target user. If the login tokens are deleted successfully, a confirmation will be returned. Otherwise, an authorization error will result. No payload is required for this endpoint.

Remove Enrollment Request:
```
DELETE host:port/registrar/lukas
```

Remove Enrollment Response:
```
{
    "OK": "Deleted login token and directory for user lukas."
}
```

The `GET /registrar/{enrollmentID}/ecert` endpoint is used to retrieve the enrollment certificate of a given user from local storage. If the target user has already registered with the CA, the response will include a URL-encoded version of the enrollment certificate. If the target user has not yet registered, an error will be returned. If the client wishes to use the returned enrollment certificate after retrieval, keep in mind that it must be URl-decoded. This can be accomplished with the QueryUnescape method in the "net/url" package.

Enrollment Certificate Retrieval Request:
```
GET host:port/registrar/jim/ecert
```

Enrollment Certificate Retrieval Response:
```
{
    "OK": "-----BEGIN+CERTIFICATE-----%0AMIIBzTCCAVSgAwIBAgIBATAKBggqhkjOPQQDAzApMQswCQYDVQQGEwJVUzEMMAoG%0AA1UEChMDSUJNMQwwCgYDVQQDEwNPQkMwHhcNMTYwMTIxMDYzNjEwWhcNMTYwNDIw%0AMDYzNjEwWjApMQswCQYDVQQGEwJVUzEMMAoGA1UEChMDSUJNMQwwCgYDVQQDEwNP%0AQkMwdjAQBgcqhkjOPQIBBgUrgQQAIgNiAARSLgjGD0omuJKYrJF5ClyYb3sGEGTU%0AH1mombSAOJ6GAOKEULt4L919sbSSChs0AEvTX7UDf4KNaKTrKrqo4khCoboMg1VS%0AXVTTPrJ%2BOxSJTXFZCohVgbhWh6ZZX2tfb7%2BjUDBOMA4GA1UdDwEB%2FwQEAwIHgDAM%0ABgNVHRMBAf8EAjAAMA0GA1UdDgQGBAQBAgMEMA8GA1UdIwQIMAaABAECAwQwDgYG%0AUQMEBQYHAQH%2FBAE0MAoGCCqGSM49BAMDA2cAMGQCMGz2RR0NsJOhxbo0CeVts2C5%0A%2BsAkKQ7v1Llbg78A1pyC5uBmoBvSnv5Dd0w2yOmj7QIwY%2Bn5pkLiwisxWurkHfiD%0AxizmN6vWQ8uhTd3PTdJiEEckjHKiq9pwD%2FGMt%2BWjP7zF%0A-----END+CERTIFICATE-----%0A"
}
```

## 6.3 CLI

The OBC CLI includes a subset of the available APIs to enable developers to quickly test and debug chaincodes or query for status of transactions. CLI is implemented in Golang and operable on multiple OS platforms. The currently available CLI commands are summarized in the following section.

### 6.3.1 CLI Commands

To see what CLI commands are currently available in the Open Blockchain implementation, execute the following:

    cd $GOPATH/src/github.com/openblockchain/obc-peer
    ./obc-peer

You will receive a response similar to below:

```
    Usage:
      openchain [command]

    Available Commands:
      peer        Run openchain peer.
      status      Status of the openchain peer.
      stop        Stop openchain peer.
      login       Login user on CLI.
      vm          VM functionality of openchain.
      chaincode   chaincode specific commands.
      help        Help about any command

    Flags:
      -h, --help[=false]: help for openchain


    Use "openchain [command] --help" for more information about a command.
```

Some of the available command line arguments for the `obc-peer` command are listed below:

* `-c` - constructor: function to trigger in order to initialize the chaincode state upon deployment.

* `-l` - language: specifies the implementation language of the chaincode. Currently, only Golang is supported.

* `-n` - name: chaincode identifier returned from the deployment transaction. Must be used in subsequent invoke and query transactions.

* `-p` - path: identifies chaincode location in the local file system. Must be used as a parameter in the deployment transaction.

* `-u` - username: registration id of a logged in user invoking the transaction.

Not all of the above commands are fully implemented in the current release. The commands that are helpful for chaincode development and debugging and are fully supported are described below.

Note, that any configuration settings for the peer node listed in `obc-peer/openchain.yaml` configuration file, which is the  configuration file for the `obc-peer` process, may be modified on the command line with an environment variable. For example, to set the `peer.id` or the `peer.addressAutoDetect` settings, one may pass the `OPENCHAIN_PEER_ID=vp1` and `OPENCHAIN_PEER_ADDRESSAUTODETECT=true` on the command line.

#### 6.3.1.1 peer

The CLI `peer` command will execute the Open Blockchain peer process in either the development or production mode. The development mode is meant for running a single peer node locally, together with a local chaincode deployment. This allows a chaincode developer to modify and debug their code without standing up a complete Open Blockchain network. An example for starting the peer in development mode is below.

```
./obc-peer peer --peer-chaincodedev
```

To start the peer process in production mode, modify the above command as follows:

```
./obc-peer peer
```

#### 6.3.1.2 login

The CLI `login` command will login a user, that is already registered with the CA, through the CLI. To login through the CLI, issue the following command, where `username` is the enrollment ID of a registered user.

```
./obc-peer login <username>
```

The example below demonstrates the login process for user `jim`.

```
./obc-peer login jim
```

The command will prompt for a password, which must match the enrollment password for this user registered with the certificate authority. If the password entered does not match the registered password, an error will result.

```
22:21:31.246 [main] login -> INFO 001 CLI client login...
22:21:31.247 [main] login -> INFO 002 Local data store for client loginToken: /var/openchain/production/client/
Enter password for user 'jim': ************
22:21:40.183 [main] login -> INFO 003 Logging in user 'jim' on CLI interface...
22:21:40.623 [main] login -> INFO 004 Storing login token for user 'jim'.
22:21:40.624 [main] login -> INFO 005 Login successful for user 'jim'.
```

#### 6.3.1.3 chaincode deploy

The CLI `deploy` command creates the docker image for the chaincode and subsequently deploys the package to the validating peer. An example is below.

```
./obc-peer chaincode deploy -p github.com/openblockchain/obc-peer/openchain/example/chaincode/chaincode_example02 -c '{"Function":"init", "Args": ["a","100", "b", "200"]}'
```

With security enabled, the command must be modified to pass an enrollment id of a logged in user with the `-u` parameter. An example is below.

```
./obc-peer chaincode deploy -u jim -p github.com/openblockchain/obc-peer/openchain/example/chaincode/chaincode_example02 -c '{"Function":"init", "Args": ["a","100", "b", "200"]}'
```

#### 6.3.1.4 chaincode invoke

The CLI `invoke` command executes a specified function within the target chaincode. An example is below.

```
./obc-peer chaincode invoke -n <name_value_returned_from_deploy_command> -c '{"Function": "invoke", "Args": ["a", "b", "10"]}'
```

With security enabled, the command must be modified to pass an enrollment id of a logged in user with the `-u` parameter. An example is below.

```
./obc-peer chaincode invoke -u jim -n <name_value_returned_from_deploy_command> -c '{"Function": "invoke", "Args": ["a", "b", "10"]}'
```

#### 6.3.1.5 chaincode query

The CLI `query` command triggers a specified query method within the target chaincode. The response that is returned depends on the chaincode implementation. An example is below.

```
./obc-peer chaincode query -l golang -n <name_value_returned_from_deploy_command> -c '{"Function": "query", "Args": ["a"]}'
```

With security enabled, the command must be modified to pass an enrollment id of a logged in user with the `-u` parameter. An example is below.

```
./obc-peer chaincode query -u jim -l golang -n <name_value_returned_from_deploy_command> -c '{"Function": "query", "Args": ["a"]}'
```


## 7. Application Model

### 7.1 Composition of an Application
<table>
<col>
<col>
<tr>
<td width="50%"><img src="images/refarch-app.png"></td>
<td valign="top">
An OBC application follows a MVC-B architecture – Model, View, Control, BlockChain.
<p><p>

<ul>
  <li>VIEW LOGIC – Mobile or Web UI interacting with control logic.</li>
  <li>CONTROL LOGIC – Coordinates between UI, Data Model and OBC APIs to drive transitions and chain-code.</li>
  <li>DATA MODEL – Application Data Model – manages off-chain data, including Documents and large files.</li>
  <li>BLOCKCHAIN  LOGIC – Blockchain logic are extensions of the Controller Logic and Data Model, into the Blockchain realm.    Controller logic is enhanced by chaincode, and the data model is enhanced with transactions on the blockchain.</li>
</ul>
<p>
For example, a Bluemix PaaS application using Node.js might have a Web front-end user interface or a native mobile app with backend model on Cloudant data service. The control logic may interact with 1 or more chaincodes to process transactions on the blockchain.

</td>
</tr>
</table>

### 7.2 7.2 Sample Application


## 8. Future Directions
### 8.1 Enterprise Integration
### 8.2 Performance and Scalability
### 8.3 Additional Consensus Plugins
### 8.4 Additional Languages


## 9. References
- [1] Miguel Castro, Barbara Liskov: Practical Byzantine fault tolerance and proactive recovery. ACM Trans. Comput. Syst. 20(4): 398-461 (2002)

- [2] Christian Cachin, Rachid Guerraoui, Luís E. T. Rodrigues: Introduction to Reliable and Secure Distributed Programming (2. ed.). Springer 2011, ISBN 978-3-642-15259-7, pp. I-XIX, 1-367

- [3] Tushar Deepak Chandra, Vassos Hadzilacos, Sam Toueg: The Weakest Failure Detector for Solving Consensus. J. ACM 43(4): 685-722 (1996)

- [4] Cynthia Dwork, Nancy A. Lynch, Larry J. Stockmeyer: Consensus in the presence of partial synchrony. J. ACM 35(2): 288-323 (1988)

- [5] Manos Kapritsos, Yang Wang, Vivien Quéma, Allen Clement, Lorenzo Alvisi, Mike Dahlin: All about Eve: Execute-Verify Replication for Multi-Core Servers. OSDI 2012: 237-250

- [6] Pierre-Louis Aublin, Rachid Guerraoui, Nikola Knezevic, Vivien Quéma, Marko Vukolic: The Next 700 BFT Protocols. ACM Trans. Comput. Syst. 32(4): 12:1-12:45 (2015)
