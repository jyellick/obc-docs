## OBC IDENTITY MANAGEMENT (MEMBERSHIP SERVICE)
&nbsp;
##### What is unique about OBC’s membership service module?
The design and implementation of the membership service module encompass many of the latest advances in cryptography, which we believe make it stand apart from other alternatives.

In addition to supporting generally-expected requirements such as preserving the privacy of transactions and making them auditable, OBC’s membership service module also introduces the concept of enrollment and transaction certificates. Infinite number of transaction certificates can be generated from their parent enrollment certificates (issued to validated users). This design ensures that asset tokens (which can be represented by transaction certificates) can only be created by verified owners, and private keys of asset tokens can be regenerated if lost. 

Furthermore, the design of the system allows transaction certificates to both expire and be revoked, which allows issuers to have greater control over the asset tokens they issued on a distributed chain.

Finally, like most other modules on OBC, you can always replace the default membership service implementation with another option.

&nbsp;
##### How and by whom is the KYC/AML (control who can open accounts on the ledger) managed?
The security module works in conjunction with the membership service module to provide access control service to any data recorded and business logic deployed on a chain network. 

When a code is deployed on a chain network, whether it is used to define a business contract or an asset, its creator can put access control on it so that only transactions issued by authorized entities will be processed and validated by chain validators.

Raw transaction records are permanently stored in the ledger. While the contents of non-confidential transactions are open to all participants, the contents of confidential transactions are encrypted with secret keys known only to their originators, and only validators and authorized auditors can interpret them.

&nbsp;
##### Would its membership service make OBC a centralized solution?
The only job of a membership service is to issue digital certificates to validated entities. It does not and should not play any role in the transaction validation process, nor should it deploy or trigger chaincodes on networks that they are issuing certificates to, so that the digital assets and codes deployed on a OBC network are still decentrally managed its users

Participants should acquire digital certificates before deploying chaincodes and issuing transactions, so that the role of the membership service can be completely isolated from transaction processing. 

