# GDG_Project :

# Secure Land Records Management System

## System Overview

This project is a decentralized application (dApp) designed to provide a secure, immutable, and transparent registry for rural land records. By leveraging Blockchain technology (EVM compatible) and InterPlanetary File System (IPFS), the system aims to prevent fraud, eliminate double-spending of land titles, and ensure verifiable ownership history without reliance on a single centralized authority.

## System Architecture

The following diagram illustrates the system's architecture, showing the flow of data between the user interface, the application's state management layer, and external decentralized services.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#2d3748', 'edgeLabelBackground':'#1a202c', 'tertiaryColor': '#1a202c', 'primaryTextColor': '#fff', 'secondaryTextColor': '#ddd'}}}%%
graph TD
    %% Define styles to match a dark, professional aesthetic similar to reference image
    classDef userStyle fill:#1a202c,stroke:#A0AEC0,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef uiStyle fill:#2d3748,stroke:#4FD1C5,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef contextStyle fill:#3e4e6a,stroke:#63B3ED,stroke-width:1px,color:#fff,rx:5px,ry:5px;
    classDef externalStyle fill:#1a202c,stroke:#F6AD55,stroke-width:2px,color:#fff,rx:5px,ry:5px;
    classDef containerStyle fill:#2D3748,stroke:#4A5568,color:#fff,rx:10px,ry:10px;

    UserIcon["User (Government Official / Citizen)"]:::userStyle

    subgraph ExternalServices["External Decentralized Services"]
        direction TB
        Wallet["Web3 Wallet Provider (e.g., MetaMask)"]:::externalStyle
        Blockchain["EVM Blockchain Network (Smart Contracts)"]:::externalStyle
        IPFS["IPFS Node (Document Storage)"]:::externalStyle
    end

    UserIcon -- "Interacts" --> FrontendUI

    subgraph AppLayer["Application Layer (Frontend Source)"]
        style AppLayer fill:#232e3e,stroke:#4A5568,color:#fff,rx:10px,ry:10px
        FrontendUI["Frontend UI (React / Next.js)"]:::uiStyle
        
        %% Connections to External via Frontend
        FrontendUI -- "Connection & Signing" --> Wallet
        Wallet -.-> Blockchain

        subgraph ContextLayer["Context & State Management Layer"]
            style ContextLayer fill:#2d3748,stroke:#63B3ED,stroke-dasharray: 5 5,color:#fff,rx:10px,ry:10px
            Providers["Context Providers"]:::contextStyle
            
            Providers --> AuthContext["AuthContext (Role & Wallet State)"]:::contextStyle
            Providers --> LandData["LandDataContext (Query & View)"]:::contextStyle
            Providers --> TxContext["TransactionContext (Mint & Transfer)"]:::contextStyle
        end

        FrontendUI -- "Manages State" --> Providers
        
        %% Context connections to external services
        TxContext -- "On-chain Settlement (Write)" --> Blockchain
        LandData -- "Fetch Documents (Read)" --> IPFS
        LandData -- "Query Ledger (Read)" --> Blockchain
    end


```

### Architectural Component Breakdown

1. **User & Wallet:** Users (Officials or Citizens) interact with the application through a Web3-injected browser extension like MetaMask, which handles cryptographic signing of transactions.
2. **Application Layer (Frontend):** Built with React.js or Next.js, this layer provides the user interface for registering land, viewing properties, and initiating transfers.
3. **Context Layer:** This internal layer manages the application's local state, handling user authentication status, fetching land data, and managing the lifecycle of blockchain transactions.
4. **External Services:**
* **EVM Blockchain:** Hosts the Solidity smart contracts that govern ownership rules, validation logic, and maintain the immutable ledger of land titles.
* **IPFS:** A peer-to-peer network used to store large deed documents, high-resolution maps, and images securely off-chain. Only the cryptographic hash (CID) of these documents is stored on the blockchain.



## Technical Requirements

To deploy and run this system effectively, the following technical stack is required.

### Software & Tools

* **Blockchain Network:** Ethereum (Sepolia/Goerli Testnet) or Polygon (Mumbai Testnet) for lower transaction costs.
* **Smart Contracts:** Solidity (v0.8.x or higher).
* **Development Framework:** Hardhat (recommended for testing and deployment scripts) or Truffle.
* **Frontend Framework:** React.js or Next.js.
* **Web3 Client Libraries:** Ethers.js or viem/wagmi for blockchain interaction.
* **Storage:** IPFS pinning service (e.g., Pinata) for reliable document handling.

### Functional Prerequisites

* **Browser:** A modern web browser (Chrome, Firefox, Brave) with a Web3 wallet extension installed.
* **Node.js:** Version 16.x LTS or higher installed on the host machine.

## Key Features

* **Immutable Records:** Once land data is written to the blockchain, it cannot be altered or deleted, providing a definitive source of truth.
* **Role-Based Access:** Smart contracts enforce permissions, ensuring only authorized Government Officials can mint new land tokens, while verified owners can initiate transfers.
* **Decentralized Storage:** Physical document digitization relies on IPFS, ensuring data availability even if centralized servers go down.
* **Transparent Audit Trail:** Every transaction, from initial registration to subsequent transfers, is publicly verifiable on the blockchain explorer.
