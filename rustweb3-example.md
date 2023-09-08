---
sidebar_position: 7
---

# Rust-Web3

Web3 is a Rust-based library for interacting with the Ethereum blockchain. Using web3, we can perform transactions with the `Finale` smart contract. Here are some examples:

## Installation

You can add web3 to your project by adding the following line to your `Cargo.toml`:

```toml
[dependencies]
web3 = "0.18.0"
tokio = { version = "1", features = ["full"] }
serde_json = "1.0"
serde_derive = "1.0"
```

## Creating a Transport and Web3 Instance

You need to create a transport to connect to the zkSync Testnet network.

```rust
let transport = web3::transports::Http::new("https://zksync2-testnet.zksync.dev")?;
let web3 = web3::Web3::new(transport);
```

By creating a transport, you can sign and send transactions. Here is an example of creating a transport using a node URL:

```rust
let my_account: Address = "YOUR_PRIVATE_KEY".parse().unwrap();
```

## Connecting to the Finale Contract

To connect to the `Finale` contract, you need the ABI and address of the contract.

```rust
let contract_address: Address = "0xF5fb2BdD82dc0f1F5886B590c6557C44f7D127aa".parse().unwrap();
let contract_abi: &'static str = r#"
[
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
"#;

let contract = Contract::from_json(
    web3.eth(),
    contract_address,
    contract_abi.as_bytes(),
)?;
```

## Calling Contract Functions

Once connected to the contract, we can call contract functions. Below are examples of how to call the `executeSwaps` functions:

### Calling the executeSwaps Function

```rust
// Set up the contract function call
let swap_params = serde_json::json!([
    {
        "poolAddress": "0x70befdbe982a2566dfd440977dba8716dcf0aa2a",
        "tokenIn": "0xadCae7cFec991Bd864055043439a103cbeF152f0",
        "tokenOut": "0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B",
        "amountIn": "1000000000000000000",
        "amountOutMin": "0",
        "swapType": "1"
    },
    {
        "poolAddress": "0x5d9aE87e0BE6047e99B40C3Dc0B47470710C811A",
        "tokenIn": "0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B",
        "tokenOut": "0xadCae7cFec991Bd864055043439a103cbeF152f0",
        "amountIn": "650000000000000000000",
        "amountOutMin": "0",
        "swapType": "2"
    }
]);
let min_total_amount_out = U256::from_dec_str("1000000000000000000").unwrap(); 

// Call the contract function
let result = contract.call("executeSwaps", (swap_params, min_total_amount_out), my_account, Options::default()).await?;

println!("{:?}", result);
```

These code snippets show you how to call the functions of the `Finale` contract using web3.
