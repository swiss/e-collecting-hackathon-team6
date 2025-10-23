# 6) Anonyme Unterstützungsbekundungen

*Over the course of two days, you will develop your solution for collecting electronic signatures for popular initiatives and referendums from A to Z, addressing the 10 topics outlined in the [guidelines](https://www.bk.admin.ch/bk/de/home/politische-rechte/e-collecting/aktuelles.html). Your prototype can be conceptual, clickable, and/or technical. Either way, you should clearly present the interactions and data flows between actors, software, and infrastructure components over time, as well as the user experience of these actors.*

## Approach

*A brief description of your approach, a link/reference to the detailed description of your approach and what you have already created (if applicable). Please also mention which skills you need for your team.*

## Documentation and Diagrams

*Together, you will contribute to comparing different ways of how to implement e-collecting in Switzerland from A to Z. As part of the [participatory process](https://www.bk.admin.ch/bk/de/home/politische-rechte/e-collecting/partizipativer_prozess.html), your solutions will be discussed in subsequent workshops and will possibly be taken into account for the official decision on the design of the federal e-collecting trials. Proper documentation is key to ensuring that your solution can be understood and evaluated:*

1. **[Mermaid](https://mermaid.js.org/) diagram(s) showing interactions and data flows between actors, software and infrastructure components of your solution over time.**
2. **Wireframes or mockups with user flow showing the user experience of different actors** (using e.g. Figma)
3. Explain how you addressed the topics presented in the [guidelines](https://www.bk.admin.ch/bk/de/home/politische-rechte/e-collecting/aktuelles.html), filling in the template below.
4. List the key strengths and weaknesses of your solution.
5. Explanation of features used (if applicable)
6. A requirements file with all packages and versions used (if applicable)
7. Environment code to be run (if applicable)

--------

# Privacy-Preserving Verifiable Hybrid E-Collecting
_A Trust-Minimized Protocol for Gradual Transition to Verifiable Electronic Signature Collection_

This proposal extends **LH15** ([link](https://e-voting.bfh.ch/app/download/6106455461/LH15.pdf?t=1609753513)), a peer-reviewed cryptographic protocol for secure and anonymous participation, to support **hybrid e-collecting**. It explicitly supports the coexistence of **traditional paper-based** and **electronic** signature collection, with strong guarantees:

- No duplicate participations
- Seamless 'upgrade' path from paper to electronic participation
- No single trusted central authorities holding secrets
- No compromise of voter privacy (Keine Gesinnungsdatenbank)

Despite its cryptographic rigor, the system remains **lightweight** and **privacy-preserving** at scale.

## Key Concept: Seamless Bootstrapping from Paper to Hybrid

The protocol enables a **simple initial deployment** that builds directly on **current paper-based processes**, then **evolves naturally** into a full hybrid system — without requiring abrupt system changes or voter behavior shifts. The eventual transition to the pure electronic form provides everlasting participation privacy.

## Bootstrapping Strategy

### 1. **Start: Paper-Only Collection Using Cryptographic Anchors**

- Each municipality associates a **secret value `γ` (gamma)** with every eligible voter.
    
- Upon receiving a valid **paper signature**, the municipality:
    
    - Encrypts the corresponding `γ` using the public encryption key `pk` for the given event.
        
    - Posts the encrypted value `f = E_pk(γ)` to the **Public Bulletin Board (PBB)** over an **authenticated channel**
        
- This step anchors each participation cryptographically — **without requiring** any **voter-side infrastructure**.
      
The system remains fully paper-based but already supports:
- **The electronic universal verifiable tallying**
- **Participation Verifiability**
- **Eligibility Verifiability**
    

### 2. **Transition: Voter Registers for Electronic Participation**

- A voter installs the official app (wallet) and opts in to electronic participation.
    
- The municipality **shares the secret `γ`** with the voter **securely**, enabling the voter to store their participation credential within the app.
    
- With  γ the voter creates the extended LH15 private tuple `(α, β, γ)`within the wallet and can compute the public credential `u = h₁^α · h₂^β · h₃^γ`.
  
- The public credential is then sent **over an authentic channel** to the municipality.
    
The voter becomes eligible to also participate electronically — using the same underlying data as in the paper-based phase.


### 3. **Full Hybrid: Dual Submission Channels**

- For signing, the voter now has an additional option. They can now create **a zero-knowledge proof of eligibility** along with an **encryption of `γ`** on their wallet and send it to the **Public Bulletin Board**:
    
    - `e = E_pk(γ)`
        
    - Plus proofs (`π₁`, `π₂`, `π₃`) as per the extended LH15 protocol
      
    - This then is sent to the **Public Bulletin Board (PBB)** over an **anonymous channel**
      
    - Only valid tuples  ((`π₁`, `π₂`, `π₃`) , e)` are accepted
        
- After the voting period, the system runs a privacy preserving **Plaintext Equality Test (PET)** between:
    
    - All encrypted `γ` values from electronic votes (`e`)
        
    - All encrypted `γ` values from paper submissions (`f`)
        
- Duplicates are detected and resolved without revealing the voter’s identity.
    
The system is now fully hybrid gaining the following properties:
- **No double participations**
- **Participation-privacy for all participants**
- **Fully verifiable**

### 4. **Electronic Only**

- If desired, the paper option can be eradicated from the system. This brings the benefit of getting rid of the 'Unterschrifte Bschiss', strengthening of participation privacy and reduction of trust. The system can drop the need for `γ` so the voter now only has to create **a zero-knowledge proof of eligibility** on their wallet and send it to the **Public Bulletin Board**:
  
    * Proofs (π₁, π₂) as per the original LH15 protocol
      
    - This then is sent to the **Public Bulletin Board (PBB)** over an **anonymous channel**
      
    - Only valid tuples  (`π₁`, `π₂`) are accepted
      
    - At any time, the actual tally is provided by all valid (`π₁`, `π₂`) tuples.
    
The system is now complete electronic gaining the following properties:
- **No double participations** even during voting period
- **Everlasting Participation-privacy for all participants**
- **Fully verifiable**
- **Simplicity**
    
## Benefits of This Bootstrapping Approach

- **No hard cutover**: Municipalities can continue paper collection with cryptographic augmentation.
    
- **Low entry barrier**: Voters need not change behavior unless they choose to.
    
- **Scalable trust model**: Voters gain full privacy and autonomy by opting in.
    
- **Continuity of existing processes**: Paper-based workflows remain compatible.
    
- **Universal verifiability**: All steps — including registration, submission, and tallying — are publicly auditable.

## Summary of Cryptographic Flow

### Credentials & Proofs (Extended LH15)

- **Voter private values**: `α`, `β`, `γ`
    
- **Public credential**: `u = h₁^α · h₂^β · h₃^γ`
    
- **Encrypted γ**: `e = E_pk(γ, t)`
    
- **Commitment**: `d = com_q(α, β, γ, s)`
    
- **Proofs**:
    
    - `π₁`: unchanged (LH15)
        
    - `π₂`: extended to include `γ`
        
    - `π₃`: proof of correct encryption of `γ`
        

### Paper Submission Digitization

- Municipality encrypts `γ` and posts:  
    `f = E_pk(γ, t)`
    
### Duplicate Detection

- **PET** between:
    
    - List `E`: electronic submissions `e`
        
    - List `F`: paper submissions `f`
        
## Conclusion

This protocol **extends the peer-reviewed LH15** with a simple cryptographic mechanism that enables municipalities to **bootstrap hybrid e-collecting** from existing paper-based workflows — with:

- **No need for immediate infrastructure change**  
- **Strong participation privacy** 
- **Universal, disputefree verifiability**
- **No trust in central authorities**
    

Voters and municipalities can transition smoothly, at their own pace, while preserving the 'feeling' of the current system.## User Experience

------------

*Add or reference wireframes or mockups with user flow showing the user experience of different actors.*

## Topics addressed

*Explain how you addressed the topics presented in the [guidelines](https://www.bk.admin.ch/bk/de/home/politische-rechte/e-collecting/aktuelles.html), filling in the template below.*

| Topic | (How) is it addressed? |
| -| ------- |
| 1 |  |
| 2 |  |
| 3 |  |
| ... |  |

## Key Strenghts and Weaknesses

*List the key strengths and weaknesses of your solution.*

### Strengths:
- ...
- ...

### Weaknesses:
- ...
- ...

## Getting Started

*These instructions will get you a copy of the technical prototype (if applicable) up and running on your local machine for development and testing purposes. **If you are not developing a technical prototype, please present or reference your conceptual and/or clickable prototype.***

### Prerequisites

*What things you need to install the software and how to install them.*

### Installation

*A step by step series of examples that tell you how to get a development env running.*

## Contributing

Please read [CONTRIBUTING.md](/CONTRIBUTING.md) for details on our code of conduct.

## Team Members

- Reto Koenig/knr1 (role)
- Philipp Locher/philoc (role)
- Name/GitHub Account (role)
- ...

## License

This software is licensed under a AGPL 3.0 License - see the [LICENSE](LICENSE) file for details. Please feel free to [choose any other](https://choosealicense.com/) [Open Source Initiative approved license](https://opensource.org/licenses) (e.g. a permissive license such as [MIT](https://opensource.org/license/mit)). Other content (e.g. text, images, etc.) is licensed under a [Creative Commons CC BY-SA 4.0 license](https://creativecommons.org/licenses/by-sa/4.0/deed.de). Exceptions are possible in consultation with the organizers.
