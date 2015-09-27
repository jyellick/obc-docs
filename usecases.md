
## B2B Contract

Business contracts can be codified to allow two or more parties to automate contractual agreements in a trusted way.  Although information on blockchain is naturally “public”, B2B contracts may require privacy control to protect sensitive business information from being disclosed to outside parties that also have access to the ledger. 

While confidential agreements are a key business case, there are many scenarios where contracts can and should be easily discoverable by all parties on a ledger. For example, a ledger used to create offers (asks) seeking for bids. This type of contract may need to be standardized so that bidders can easily find them, effectively creating an electronic trading platform with smart contracts (aka chaincode).

#### Persona

*  Contract participant – Contract counter parties

*  Third party participant – A third party stakeholder of a contract who plays the role of guaranteeing the integrity of the contract.

#### Key Components

*  Multi-sig contract activation - When a contract is first deployed by one of the counter parties, it will be in pending activation state. To active a contract, signatures from other counterparties and/or third party participants are required.

*  Multi-sig contract execution - Some contract will require one of many signatures to execute. (E.g. In trade finance, a payment instruction can only be executed if either the recipient or a authorized third party (e.g. UPS) confirms the shipment of the good)

*  Discoverable contract offers - If a contract is a business offer seeking for counter parties (bids), it must also be made searchable. In addition, such contract shall have the intelligence to evaluate bids.

*  Payment inside contract- Integration with off-chain payment system is necessary so that contract code can trigger payment transactions that are settled off chain

*  Atomicity of contract execution - Atomicity of the contract is needed to guarantee asset transfers can only occur when payment is received (Delivery vs. Payment). If any step in the execution process fails, the entire transaction must be rolled back

*  Contract to chain-code communication - Contracts must be able to communicate with chaincodes deployed on the ledger. E.g. Security lending contracts will call asset contracts to deliver digital assets to counter parties

*  Longer Duration contract - Timer is needed to because many B2B contracts may have long execution window.

*  Reuseable contracts - Oftenly used contracts can be standardized so that they can be easily reused. In addition, standardization will improve searchability of contracts.

*  Auditable contractual agreement -  Any contract can be made auditable to third parties. E.g. Lawyers may want to audit transactions for legal reasons

*  Contract life-cycle management - B2B contracts are unique and not all of them can be standardized. An efficient contract management system is needed so that contracts will not bloat the system

*  Validation access – only nodes with validation right are allowed to validate transactions of a B2B contract, and that could be restricted to stake holders and authorized third party

*  View access – B2B contracts may include confidential information, so only accounts with view access right are allowed to view/interrogate them

 


## Manufacturing Supply Chain

Final assemblers, such as car manufacturers, can create a supply chain network managed by its peers and suppliers so that a final assembler can better manage its suppliers and be more responsive to events that would require vehicle recalls (possibly triggered by faulty parts supplied by some supplier). The blockchain fabric must provide a standard protocol to allow every participant on a supplychain network to input and track parts produced and used on a given vehicle.

Why is this specific example an abstract use case? Because while all blockchain cases store immutable information, and some add the need for transfer of assets between parties, this case emphasizes the need to provide deep searchability back as many as 5-10 transaction layers. It is the core of establishing provenance of any manufactured good that is made up of other goods and supplies.

## Asset Depository 

Assets such as financial securities must be able to be dematerialized on a blockchain network so that all stakeholders of an asset type will have direct access to that asset, allowing them to initiate trades and acquire information on an asset without going through layers of intermeidaries. Trades should be settled in near real time and all stakeholders must be able to access asset information in near real time. A stakeholder should be able to add business rules on any given asset type, further reducing operating cost with automation logic.
