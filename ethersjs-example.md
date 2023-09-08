---
sidebar_position: 4
---

# Ethers.js

Ethers.js is a JavaScript-based library for interacting with the Ethereum blockchain. Using ethers.js, we can perform transactions with the `Finale` smart contract. Here are some examples:

## Installation

You can install ethers.js using npm or yarn. You can add ethers.js to your project with the following command:

```bash
npm install --save ethers
```
or
```bash
yarn add ethers
```

## Import

You can include ethers.js in your project using the following line:

```javascript
const ethers = require('ethers');
```

## Creating a Provider and Signer

You need to create a provider to connect to the zkSync Testnet network. 

```javascript
let provider = new ethers.providers.JsonRpcProvider('https://zksync2-testnet.zksync.dev');
```

By creating a signer, you can sign and send transactions. Here is an example of creating a signer using a private key:

```javascript
let privateKey = "YOUR_PRIVATE_KEY";
let wallet = new ethers.Wallet(privateKey);
let signer = wallet.connect(provider);
```

## Connecting to the Finale Contract

To connect to the `Finale` contract, you need the ABI and address of the contract.

```javascript
let finaleContractAddress = "0xF5fb2BdD82dc0f1F5886B590c6557C44f7D127aa";
// Just executeSwaps params in here
let finaleContractABI = [    
    {
      "inputs": [
        {
          "components": [
            {
              "internalType": "address",
              "name": "poolAddress",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "tokenIn",
              "type": "address"
            },
            {
              "internalType": "address",
              "name": "tokenOut",
              "type": "address"
            },
            {
              "internalType": "uint256",
              "name": "amountIn",
              "type": "uint256"
            },
            {
              "internalType": "uint256",
              "name": "amountOutMin",
              "type": "uint256"
            },
            {
              "internalType": "uint256",
              "name": "swapType",
              "type": "uint256"
            }
          ],
          "internalType": "struct Params.SwapParam[]",
          "name": "swapParams",
          "type": "tuple[]"
        },
        {
          "internalType": "uint256",
          "name": "minTotalAmountOut",
          "type": "uint256"
        }
      ],
      "name": "executeSwaps",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    }
];

let finaleContract = new ethers.Contract(finaleContractAddress, finaleContractABI, signer);
```

## Calling Contract Functions

Once connected to the contract, we can call contract functions. Below are examples of how to call the `executeSwaps` functions:

### Calling the executeSwaps Function

```javascript
let swapParams = [
    {
        amountIn: ethers.utils.parseUnits("1", 18),
        amountOutMin: ethers.constants.AddressZero,
        poolAddress: "0x70befdbe982a2566dfd440977dba8716dcf0aa2a", 
        swapType: 1, 
        tokenIn: "0xadCae7cFec991Bd864055043439a103cbeF152f0",
        tokenOut: "0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B"
    },
    {
        amountIn: ethers.utils.parseUnits("650", 18),
        amountOutMin: ethers.constants.AddressZero,
        poolAddress: "0x5d9aE87e0BE6047e99B40C3Dc0B47470710C811A", 
        swapType: 2, 
        tokenIn: "0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B",
        tokenOut: "0xadCae7cFec991Bd864055043439a103cbeF152f0"
    }
];
let minTotalAmountOut = ethers.utils.parseEther("1"); 

let tx = await finaleContract.executeSwaps(swapParams, minTotalAmountOut);
let receipt = await tx.wait();
console.log(receipt);
```

These code snippets show you how to call the functions of the `Finale` contract using ethers.js.
