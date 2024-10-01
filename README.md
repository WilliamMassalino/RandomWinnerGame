# RandomWinnerGame - Smart Contract

## Table of Contents
- [Introduction](#introduction)
- [How it Works](#how-it-works)
- [Installation and Setup](#installation-and-setup)
- [Usage](#usage)
- [Features](#features)
- [Contract Variables](#contract-variables)
- [Functions Overview](#functions-overview)
- [Dependencies](#dependencies)
- [Configuration](#configuration)
- [Examples](#examples)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Introduction

RandomWinnerGame is a smart contract designed as a lottery-style game where a random winner is chosen among participants using Chainlink VRF (Verifiable Random Function) for secure and tamper-proof randomness. The contract is built on the Ethereum blockchain and uses the Chainlink oracle service to guarantee that randomness is verifiable and secure.

## How it Works

1. **Game Creation**: The owner of the contract can start a new game specifying the maximum number of players and the entry fee.
2. **Player Participation**: Players can join the game by sending the entry fee to the contract.
3. **Winner Selection**: Once the maximum number of players is reached, Chainlink VRF is called to generate a random number securely. This number is used to select a winner among the players.
4. **Payout**: The winner receives all the Ether collected in the game.

Chainlink VRFs provide verifiable randomness by using a cryptographic proof that the random values generated are secure and fair.

First, clone the project repository:
```git
git clone https://github.com/WilliamMassalino/RandomWinnerGame.git
```

## Installation and Setup

### Prerequisites
- **Foundry**: To compile and deploy the contract.
- **Node Provider**: [QuickNode](https://www.quicknode.com/) is recommended to connect to the Sepolia testnet.
- **Metamask**: For accessing your Ethereum wallet.
- **Sepolia ETH**: Get some Sepolia ETH from a faucet for testing.
- **Chainlink LINK Tokens**: To fund the contract for requesting randomness.

### Setting Up Environment Variables
1. Create a `.env` file in your project directory.
2. Add the following variables:
```env
PRIVATE_KEY="..."         # Your wallet private key (for test account)
QUICKNODE_RPC_URL="..."   # QuickNode RPC URL for the Sepolia network
ETHERSCAN_API_KEY="..."   # Etherscan API key
```
3. Load environment variables:
```bash
source .env
```

### Compiling and Deploying the Contract

#### Compile the contract using Foundry:

```bash
forge compile
```

### Deploy the Contract to Sepolia

```bash
forge create --rpc-url $QUICKNODE_RPC_URL --private-key $PRIVATE_KEY --etherscan-api-key $ETHERSCAN_API_KEY --verify src/RandomWinnerGame.sol:RandomWinnerGame
```
**If There Are Issues with Contract Verification**
```bash
forge verify-contract <contract_address> <contract_name> --chain <chain_name> --watch
```
## Usage

1. **Funding the Contract**: Use the [Chainlink Faucet](https://faucets.chain.link/sepolia) to send LINK tokens to your contract address to fund randomness requests.
2. **Starting the Game**: Connect your wallet to the deployed contract on Etherscan, then call `startGame` with your desired number of maximum players and entry fee.
3. **Joining the Game**: Players can join by calling `joinGame` and sending the entry fee in Ether.
4. **Random Winner Selection**: When the maximum number of players is reached, the contract requests randomness from Chainlink VRF. The winner is selected and the Ether balance is transferred to the winner's address.
5. **Viewing Results**: Check the events emitted by the contract to see the results, including `GameStarted`, `PlayerJoined`, and `GameEnded`.

## Features

- **Lottery Game**: Host a simple lottery game with customizable entry fees and player limits.
- **Random Winner Selection**: Uses Chainlink VRF to select a winner in a verifiably random manner.
- **On-Chain Events**: Emits events for game creation, player participation, and game ending.
- **Secure Payments**: Winner automatically receives the accumulated entry fees in Ether.

## Contract Variables

- **`fee`**: LINK token fee for randomness requests.
- **`keyHash`**: The public key for VRF.
- **`callbackGasLimit`**: Gas limit for VRF callback.
- **`requestConfirmations`**: Number of confirmations for randomness request.
- **`numWords`**: Number of random words required (set to 1).
- **`linkAddress`**: LINK token contract address (Sepolia).
- **`wrapperAddress`**: VRF wrapper contract address (Sepolia).
- **`players`**: Array of addresses of players in the game.
- **`maxPlayers`**: Maximum number of players allowed in a game.
- **`gameStarted`**: Boolean indicating if a game is in progress.
- **`entryFee`**: Fee for entering the game.
- **`gameId`**: Unique identifier for each game.

## Functions Overview

### Game Management
- **`startGame(uint8 _maxPlayers, uint256 _entryFee)`**: Starts a new game. Only the contract owner can call this.
- **`joinGame()`**: Allows a player to join the game by sending the entry fee.

### Randomness and Winner Selection
- **`getRandomWinner()`**: Initiates a request for randomness to select a winner.
- **`requestRandomWords()`**: Submits a request to Chainlink VRF for random number generation.
- **`fulfillRandomWords(uint256 _requestId, uint256[] memory _randomWords)`**: Callback function that handles the random value returned by Chainlink and selects a winner.

### Events
- **`GameStarted(uint256 gameId, uint8 maxPlayers, uint256 entryFee)`**: Emitted when a game is started.
- **`PlayerJoined(uint256 gameId, address player)`**: Emitted when a player joins the game.
- **`GameEnded(uint256 gameId, address winner, uint256 requestId)`**: Emitted when the game ends and a winner is chosen.

### Ether Management
- **`receive()` & `fallback()`**: Functions to receive Ether sent to the contract.

## Dependencies

- **Chainlink VRF**: For secure and verifiable random number generation.
- **ConfirmedOwner**: A utility contract that restricts certain functions to the contract owner.
- **LinkTokenInterface**: Interface for interacting with the LINK token.

## Configuration

- **Sepolia Network**: The contract is set up for the Sepolia testnet. Ensure you are working within the correct network context.
- **LINK and WRAPPER Addresses**: These are hardcoded for Sepolia, modify them accordingly if deploying to another network.
- **Gas Limits and Fees**: Customize `callbackGasLimit`, `requestConfirmations`, and `numWords` based on your needs.

## Examples

- **Start a Game**:
  - Call `startGame(2, 1000000000000000)` to start a game with 2 players and an entry fee of 0.001 ETH.
  
- **Join a Game**:
  - Call `joinGame()` from a player account and send 0.001 ETH.
  
- **Game Ends**:
  - When the maximum number of players is reached, a random winner is selected and the prize is sent.

## Troubleshooting

- **Insufficient LINK Balance**: Ensure the contract is funded with enough LINK tokens to request randomness.
- **Gas Limits**: Adjust `callbackGasLimit` if your transactions are failing due to insufficient gas.
- **No Events Emitted**: If `GameEnded` is not emitted, check the Chainlink VRF status and verify that randomness is being fulfilled correctly.

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.




