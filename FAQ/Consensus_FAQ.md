## OBC CONSENSUS ALGORITHM

&nbsp;
##### Which Consensus Algorithm is used in OBC? 
OBC is built on a pluggable architecture such that developers can configure their deployment with the consensus module that best suits their needs. The initial OBC release package will offer three consensus implementations for users to select from: 1) No-op (consensus ignored); 2) Classic PBFT; and 3) SIEVE (an enhanced version of classic PBFT). 

&nbsp;
##### What is the SIEVE Consensus Algorithm?
There are many limitations in existing consensus algorithms, especially for solving challenges around security, performance, efficiency, and scalability. To solve these problems and make blockchain ready for business, the OBC team has been researching several approaches to improve the existing algorithms. 

One such improvement is “SIEVE”, a consensus algorithm inspired by classic PBFT [Castro and Liskov, OSDI’99] and the Eve consensus protocol [Kapritsos et al., OSDI’2012]. 

In short, SIEVE augments the original PBFT algorithm by adding speculative execution and verification phases to: 1) detect and filter out possible non-deterministic requests and establish the determinism of transactions entering the PBFT 3-phase agreement protocol, and 2) allow consensus to be run on output state of validators, in addition to the consensus on their input state offered by Classic PBFT. SIEVE is derived from PBFT in a modular way (inspired by ideas described in  [Aublin et al., TOCS'15]) by reusing the PBFT view-change protocol to lower its complexity and avoid implementing a new consensus protocol from scratch.

A research paper will be published by IBM Research to describe this new algorithm in further detail, and we will continue to make further research investments in the subject and share our results with the OBC community.

&nbsp;
##### What are the expected performance figures for OBC?
The performance of a chain network depends on several factors, such as the encryption method used, transaction message size, security level set, business logic running, number of validators, proximity of the validating nodes, and the consensus algorithm deployed, etc.

The current performance goal for OBC is to achieve 100,000 transactions per second in a standard production environment of approximately 15 validating nodes running in close proximity. The OBC team is committed to continue to invest in improving the performance and the scalability of the system.
