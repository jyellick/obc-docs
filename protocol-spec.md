# Open Ledger Protocol Specification

_Draft 0.01_

## Preface
This document is not intended to be a complete explanation of the Open Ledger implementation, but a protocol specification for understanding the interface between the pluggable components of the system such that one may build a compatible system.

### Intended Audience
The intended audience for this specification includes the following groups:

* Blockchain vendors who want to implement blockchain systems that conform to this specification
* Tool developers who want to extend the capabilities of Open Ledger
* Application developers who want to leverage blockchain technologies to enrich their applications

### Authors
These are the authors who wrote various sections of this document:  Binh Q Nguyen, David Kravitz, Elli, Angelo, Marko, Sheehan Anderson

_add your name if you write any sections of the doc_

### Reviews 
Frank Lu, John Wolpert, Bishop Brock, Nitin Gaur, Sharon Weed
DAH folks, LSEG folks
_add your name if you contribute to the doc_


### Acknowledgements
List those who influenced the project: Jerry, John c, Joe Latone, Christian C, Donna, 

## Table of Contents
1. [Introduction](#introduction)

   - 1.1 What is Open Ledger?
   - 1.2 Why Open Ledger?
   - 1.3 Terminology

3. [Fabric](#fabric)
  
4. [Protocol](#protocol)
   - Messages: Handshake, Transact, Sync
	Transaction Types and Structures
		Life-cycle: Publish, Create, Invoke
	Block structure
	World State
	Chaincode (communication, api, execution, limit, determinism)
	Consensus Framework and Pluggability

5. [Consensus](#consensus)
   - Practical Byzantine design and implementation
	- Addressing determinism (assumption, detection, action)

6. [Execution Model](#executionmodel)
   - Transaction flow
	Execution and state modification
	Error codes

7. [Security](#security)
   - Introduction to security model
	Scenarios and goals
	Network security (TLS)
	Registration ECA and TCA
	Transaction security (privacy, confidentiality, signature, multisig)
	Business security (audit, linkability, reputation)
	Chaincode Access Control
	Application Access Control (applications may implement own acl)

8. [Application Programming Interface](#api)
   - HTTP Service (security, topology)
	REST APIs (description, usage, sample, swagger)
	CLIs (motivations, usage, security)

9. [Application Model](#applicationmodel)
   - Composition of an application
	Samples to illustrate

10. [Future Directions](#futuredirections)
 
11. [References](#references)
   - bitcoin
	pbft
	ethereum


## 1. Introduction
This document specifies the principles, architecture, and protocol of Open Ledger, a blockchain suitable for industrial use-cases.

### 1.1 What is Open Ledger?
Open Ledger is a ledger of digital events shared among  different participants, each has a stake in the system. The ledger can only be updated by consensus of a majority of the participants, and, once recorded, information can never be altered. Each recorded event is cryptographically verifiable with proof of agreement from the parties.


### 1.2 Why Open Ledger?

Blockchain technology is in its infancy and is often not well suited for the needs of industry. Scalability challenges and the lack of support for confidential and private transactions, among other issues, make its use infeasible for many important industry applications. Open Ledger is an industry–focused design, based on and extending the learnings of the pioneers in this field.

Open Ledger is designed for permissioned networks, where participants are registered members of the system. It allows compliance with regulations and respects the requirements that arise when competing businesses work together on the same network.

### 1.3 Terminology
##### Transaction
Transaction is a request to the blockchain to execute a function on the ledger
##### Transactor
Transactor is an entity that issues transactions such as client application
##### Ledger
Ledger is a sequence of cryptographically linked blocks, containing transactions and current state
##### State
Executing transactions may be persisted in variables called state. The ledger contains a number of configurable delta states between blocks and current state
##### Chaincode
Chaincode is an application-level code (a.k.a smart contract) stored on the ledger part of a transaction. Chaincode runs transactions and persists state
##### Validating Node
A computer on blockchain network responsible for running consensus and maintaining the ledger
##### Non-validating Node
A computer node functions as a proxy connecting transactors to the validating node. Non-validating node provides event stream and hosts REST API service, so it responds to most of API requests
##### Permissioned Ledger
Each entity or node on the blockchain network is required to be a member of the network. Anonymous nodes are not allowed to connect
##### Privacy
Transactors need privacy to conceal their identities on the network. While members of the network may examining the transactions, but the transactions can't be linked to the transactor without special privilege
#####Confidentiality
Confidentiality is the ability to render the transaction content inaccessible to anyone other than the stakeholders of the transaction
##### Auditability
While private transactions are important, business usage of blockchain needs to comply with regulations and make it easy for regulators to investigate transaction records
##### Finality
Data stores for industrial use must provide finality, meaning simply that there must be certainty at what point a transaction is committed, and that after this point, it will not change on its own. Many current blockchain networks offer only eventual consistency, rather than a firm promise of finality, and can suffer arbitrary loss of committed information as they are designed to allow for, and cope with, forks in their global state.


## 2. Fabric

The fabric is made up of the core components described in the subsections below. 

### 2.1 Architecture
Figure 1 shows the reference architecture aligned in 3 categories: Membership, Blockchain, and Chaincode services. These categories are a logical structure, not a physical depiction of partitioning of components into separate processes, address spaces or (virtual) machines.

![Reference architecture](images/refarch.png)
Figure 1:  Openchain Reference architecture

##### Membership Services
Membership provides services for managing identity, privacy, and confidentiality on the network. Participants register to get identities, which will enable the Attribute Authority to issue security keys to transact. Reputation Manager enables auditors to see transactions pertaining to a participant. Of course, auditors will have to be granted proper access authority by the participants.

##### Blockchain Services
Blockchain services manage the distributed ledger through a peer-to-peer protocol, built on HTTP/2. The data structures are highly optimized to provide the most efficient hash algorithm for maintaining the world state replication. Different consensus (PBFT, Raft, PoW, PoS) may be plugged in and configured per deployment.

##### Chaincode Services
Chaincode services provides a secured and lightweight way to sandbox the chaincode execution on validating nodes. The environment is a “locked down” and secured container along with a set of signed base images containing secure OS and chaincode language, runtime and SDK layers for Golang, Java, and Node.js. Other languages can be enabled if required.

##### Event Stream

##### Rest API

##### CLI

### 2.2 Topology
A deployment of Open Ledger may consist of a member service, many validating nodes, non-validating nodes, and application client. All makes up a chain. There can be multiple chains; each has own operating parameters and security concerns.

_TODO: Diagram show connections between entities_

There are 3 potential deployment models: Cloud hosted 1 network, cloud hosted multiple networks, or within each participant’s intranet.

The simplest and most efficient topology is cloud hosted 1 network, where each participant owns a number of nodes, including validating and non-validating. Even though the network is in cloud, hosted by a vendor, who owns the physical boxes, the participants contractually control the computing resources, making it decentralized within a centralized environment.

Cloud hosted multiple networks allow participants to have their nodes hosted by any cloud providers, given that nodes can connect to one another over HTTPs.

Similar to cloud hosted multiple networks, using participants’ own networks is also possible via HTTPs channel.


## 3. Protocol
   - Messages: Handshake, Transact, Sync
	Transaction Types and Structures
		Life-cycle: Publish, Create, Invoke
	Block structure
	World State
	Chaincode (communication, api, execution, limit, determinism)
	Consensus Framework and Pluggability


## 4. Consensus
   - Practical Byzantine design and implementation
	- Addressing determinism (assumption, detection, action)

## 5. Execution Model
   - Transaction flow
	Execution and state modification
	Error codes

## 6. Security
   - Introduction to security model
	Scenarios and goals
	Network security (TLS)
	Registration ECA and TCA
	Transaction security (privacy, confidentiality, signature, multisig)
	Business security (audit, linkability, reputation)
	Chaincode Access Control
	Application Access Control (applications may implement own acl)

## 7. Application Programming Interface
   - HTTP Service (security, topology)
	REST APIs (description, usage, sample, swagger)
	CLIs (motivations, usage, security)

Openchain includes REST and JSON RPC APIs, events, and an SDK for applications to communicate with the network. Typically applications interact with a peer node, which will require some form of authentication to ensure the entity has proper privilege, so messages from a client are signed by the client identity and verified by the peer node.


![Reference architecture](images/refarch-api.png) <p>
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

When you are ready to start interacting with the Openchain peer node through the available APIs and packages, follow the instructions on the [API Documentation](https://github.com/openblockchain/obc-peer/blob/master/docs/Openchain%20API.md) page.

## 8. Application Model
<table>
<col>
<col>
<tr>
<td width="50%"><img src="images/refarch-app.png"></td>
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


## 9. Future Directions
 
## 10. References
- [1] Miguel Castro and Barbara Liskov; [Practical Byzantine Fault Tolerance] (http://www.pmg.lcs.mit.edu/papers/osdi99.pdf)
- [2] [Wikipedia Smart Contract](https://en.wikipedia.org/wiki/Smart_contract)

