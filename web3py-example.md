---
sidebar_position: 5
---

# Web3.py

Web3.py is a Python library for interacting with the Ethereum blockchain. Using Web3.py, we can perform transactions with the `Finale` smart contract. Here are some examples:

## Installation

You can install Web3.py using pip. You can add Web3.py to your project with the following command:

```bash
pip install web3
```

## Import

You can include Web3.py in your project using the following line:

```python
from web3 import Web3
```

## Creating a Provider

You need to create a provider to connect to the zkSync Testnet network. 

```python
provider_url = 'https://zksync2-testnet.zksync.dev'
web3 = Web3(Web3.HTTPProvider(provider_url))
```

## Setting the Default Account

By setting the default account, you can sign and send transactions. Here is an example of setting the default account using a private key:

```python
private_key = "YOUR_PRIVATE_KEY"
web3.eth.defaultAccount = web3.eth.Account.privateKeyToAccount(private_key).address
```

## Connecting to the Finale Contract

To connect to the `Finale` contract, you need the ABI and address of the contract.

```python
Finale_Contract_Address = "0xF5fb2BdD82dc0f1F5886B590c6557C44f7D127aa"
Finale_Contract_ABI = [    
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
]

Finale_Contract = web3.eth.contract(address=Finale_Contract_Address, abi=Finale_Contract_ABI)
```

## Calling Contract Functions

Once connected to the contract, we can call contract functions. Below are examples of how to call the `executeSwaps` functions:

### Calling the executeSwaps Function

```python
def executeSwaps(swap_params: list, min_total_amount_out: int):
    tx_hash = Finale_Contract.functions.executeSwaps(swap_params, min_total_amount_out).transact()
    tx_receipt = web3.eth.waitForTransactionReceipt(tx_hash)
    print(tx_receipt)

# Call the function
swap_params = [
    {
        'amountIn': web3.toWei(1, 'ether'),
        'amountOutMin': 0,
        'poolAddress': "0x70befdbe982a2566dfd440977dba8716dcf0aa2a",
        'swapType': 1,
        'tokenIn': "0xadCae7cFec991Bd864055043439a103cbeF152f0",
        'tokenOut': "0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B"
    },
    {
        'amountIn': web3.toWei(650, 'ether'),
        'amountOutMin': 0,
        'poolAddress': "0x5d9aE87e0BE6047e99B40C3Dc0B47470710C811A",
        'swapType': 2,
        'tokenIn': "0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B",
        'tokenOut': "0xadCae7cFec991Bd864055043439a103cbeF152f0"
    }
]
min_total_amount_out = 0
executeSwaps(swap_params, min_total_amount_out)
```

These code snippets show you how to call the functions of the `Finale` contract using Web3.py.
