
## B2B Contract

Business contracts can be codified to allow two or more parties to automate contractual agreements in a trusted way.  Although information on blockchain is naturally “public”, B2B contracts may require privacy control to protect sensitive business information from being disclosed to outside parties that also have access to the ledger. 

<table>
<tr><td><img src="images/b2bcontract.png" height="200" width="500"></td></tr>
</table>

While confidential agreements are a key business case, there are many scenarios where contracts can and should be easily discoverable by all parties on a ledger. For example, a ledger used to create offers (asks) seeking for bids. This type of contract may need to be standardized so that bidders can easily find them, effectively creating an electronic trading platform with smart contracts (aka chaincode).


<table>
<tr><td><img src="images/exchange.png" height="200" width="500"></td></tr>
</table>


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

<table>
<tr><td><img src="images/supplychain.png" height="200" width="500"></td></tr>
</table>

#### Persona

*  Final Assembler – The business entity that performs the final assembly of a product.

*  Part supplier – Suppliers of parts that are used by assemblers. Each supplier can also be an assembler itself, assembled parts it received from sub-suppliers and send the finished product to final assembler

#### Key Components

*  Payment upon delivery of goods - Integration with off-chain payment system is necessary to instruct payment instructions upon delivery of a component to the assembler above.

*  Third party Audit -  All supplied parts shall be auditable by third parties. E.g. regulators may need to track total number of parts supplied by a supplier for tax accounting purpose

*  Obfuscation of shipments - Balances shall be obfuscated so that no one supplier can deduce the exact amount other supplier supplies

*  Obfuscation of market size - Total balance must be obfuscated so that part suppliers can’t deduce its market share and use that to seek for better term.

*  Validation Access – only nodes with validation right are allowed to validate transactions (shipment of parts)

*  View access – only accounts with view access right are allowed to interrogate balances of parts shipped & available.

 
## Asset Depository 

Assets such as financial securities must be able to be dematerialized on a blockchain network so that all stakeholders of an asset type will have direct access to that asset, allowing them to initiate trades and acquire information on an asset without going through layers of intermeidaries. Trades should be settled in near real time and all stakeholders must be able to access asset information in near real time. A stakeholder should be able to add business rules on any given asset type, further reducing operating cost with automation logic.

<table>
<tr><td><img src="images/assetrepository.png" height="300" width="500"></td></tr>
</table>

#### Persona

*  Investor – Beneficial owner of securities. Openchain identity manager associates each key pair to a validated identity. Through identity manager, investors can also control and manage the delivery/payment instructions linked to their identity.

*  Issuer – Business entity that issued the security which is now dematerialized on the ledger network.

*  Custodian – Hired by investors for the safekeeping of their assets, and for providing other services such as asset servicing and optimization. Note that

*  Securities Depository – Depository of dematerialized assets (securities). Known as CSD (Central Securities Depository) in the financial market.  

#### Key Components

*  Asset to cash - Integration with off-chain payment system is necessary so that in the case of fixed income securities, issuers can make coupon payments to its investors.

*  Reference Rate - Some types of assets such as floating rate notes require access to the reference rates (e.g. LIBOR), such information must be feed into the ledger network.

*  Asset Timer - A timer is needed for many types of financial assets that has a life span and have to make periodic payments. E.g. fixed-income securities need to pay periodic payments its investors and need to know what to do do when it reaches maturity.

*  Asset Auditor -  Assets must be made auditable to third parties. E.g. regulator may want to audit transactions and movements of assets to measure market risks.

*  Obfuscation of account balances - Individual account balances shall be obfuscated so that no one can deduce the exact amount an investor owns.

*  Delegation of Control - In case of financial securities, assets (securities) are still registered under each investor’s name (instead of going through a nominee). However, custodian banks will have full control over its managed assets as long as they have access to their clients' keys.

*  Validation Access – only nodes with validation right are allowed to validate transactions updating the balances of an asset type (this could be restricted to CSD and/or issuer).

*  View access – only accounts with view access right are allowed to interrogate the chaincode that defines an asset type. If an asset represents shares of a publicly traded companies, then the view access could be granted to everyone on the network.

