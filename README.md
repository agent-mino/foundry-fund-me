# **FundMe Smart Contract 📜**

A decentralized crowdfunding smart contract that allows users to fund a project with Ethereum, ensuring contributions meet a minimum value (denominated in USD) using Chainlink price feeds. The contract owner can withdraw funds securely.

---

## **Table of Contents**
- [Features](#features)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [How It Works](#how-it-works)
  - [Funding](#funding)
  - [Withdrawing Funds](#withdrawing-funds)
- [Testing](#testing)
- [Deployment](#deployment)
- [Security Considerations](#security-considerations)
- [License](#license)

---

## **Features**
- Secure crowdfunding using **Ethereum**.
- Minimum funding threshold enforced in USD using **Chainlink Price Feeds**.
- **Owner-only** withdrawal mechanism with optimized gas usage.
- Supports **fallback** and **receive** functions to handle direct ETH transfers.
- Modularized architecture with reusable `PriceConverter` library.

---

## **Getting Started**

### **Prerequisites**
- Node.js and npm installed.
- Foundry installed. (Follow the [Foundry Installation Guide](https://book.getfoundry.sh/getting-started/installation))
- An Ethereum testnet or local blockchain (e.g., Hardhat, Anvil).

### **Installation**
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/fundme-contract.git
   cd fundme-contract
   ```

2. Install dependencies:
   ```bash
   forge install
   ```

3. Compile the contract:
   ```bash
   forge build
   ```

---

## **How It Works**

### **Funding**
- Users can fund the contract by calling the `fund()` function or by sending ETH directly (via fallback/receive).
- The contract ensures that contributions meet the **minimum threshold** (`5 USD`), converted from ETH using a Chainlink price feed.

### **Withdrawing Funds**
- Only the owner of the contract can withdraw funds.
- Funds are distributed securely using the **call method** to prevent reentrancy attacks.
- Two withdrawal methods are available:
  - `withdraw`: Default withdrawal logic.
  - `cheaperWithdraw`: Optimized for gas efficiency.

---

## **Testing**

### **Running Tests**
1. Run unit tests:
   ```bash
   forge test
   ```

2. To debug specific tests with verbose output:
   ```bash
   forge test --match-test testFunctionName -vvvvv
   ```

### **Test Coverage**
The project includes tests for:
- Contract deployment (`setUp`).
- Funding functionality.
- Withdrawal security and gas optimization.

---

## **Deployment**

### **To Deploy Locally**
1. Start a local blockchain (e.g., Anvil):
   ```bash
   anvil
   ```

2. Deploy the contract using Foundry's scripting:
   ```bash
   forge script script/DeployFundMe.s.sol --broadcast --private-key <PRIVATE_KEY>
   ```

### **To Deploy on Testnets**
1. Set up environment variables in `.env`:
   ```env
   PRIVATE_KEY=<your_private_key>
   RPC_URL=<your_rpc_url>
   ```

2. Deploy to a testnet:
   ```bash
   forge script script/DeployFundMe.s.sol --rpc-url $RPC_URL --broadcast --private-key $PRIVATE_KEY
   ```

---

## **Security Considerations**
- The `onlyOwner` modifier ensures only the contract owner can withdraw funds.
- The `require` and `revert` statements protect against invalid contributions.
- Gas optimization in `cheaperWithdraw` reduces costs for high-volume operations.
- Uses the **call method** for ETH transfers to mitigate reentrancy vulnerabilities.

---

## **Contract Details**

### **Key Variables**
- `MINIMUM_USD`: The minimum funding threshold (5 USD).
- `i_owner`: The immutable owner of the contract.
- `s_addressToAmountFunded`: Tracks user contributions.

### **Key Functions**
- **Funding**:
  - `fund()`: Allows users to contribute ETH.
- **Withdrawal**:
  - `withdraw()`: Allows the owner to withdraw all funds.
  - `cheaperWithdraw()`: Optimized version for gas efficiency.
- **Getters**:
  - `getAddressToAmountFunded(address)`: Returns the funded amount for a user.
  - `getOwner()`: Returns the owner of the contract.

---

## **License**

This project is licensed under the **MIT License**. See the [LICENSE](./LICENSE) file for details.

---

### **Contributions**
Feel free to open issues and pull requests to enhance the functionality of this project.

---

### **Acknowledgments**
- Built with ❤️ and Solidity.
- Powered by **Chainlink Price Feeds**.

---

## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
