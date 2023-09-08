---
sidebar_position: 6
---

# Go-ethereum

Go-ethereum (or geth) is a Go library for interacting with the Ethereum blockchain. Using go-ethereum, we can perform transactions with the `Finale` smart contract. Here are some examples:

## Installation

You can install go-ethereum using go get. You can add go-ethereum to your project with the following command:

```bash
go get github.com/ethereum/go-ethereum
```

## Import

You can include go-ethereum in your project using the following line:

```go
import "github.com/ethereum/go-ethereum/ethclient"
```

## Creating a Client

You need to create a client to connect to the zkSync Testnet network. 

```go
client, err := ethclient.Dial("https://zksync2-testnet.zksync.dev")
if err != nil {
    log.Fatalf("Failed to connect to the Ethereum client: %v", err)
}
```

## Connecting to the Finale Contract

To connect to the `Finale` contract, you need the address of the contract.

```go
contractAddress := common.HexToAddress("0xF5fb2BdD82dc0f1F5886B590c6557C44f7D127aa")
instance, err := store.NewStore(contractAddress, client)
if err != nil {
    log.Fatalf("Failed to instantiate a contract: %v", err)
}
```

## Calling Contract Functions

Once connected to the contract, we can call contract functions. Below are examples of how to call the `executeSwaps` functions:

### Calling the executeSwaps Function

```go
swapParams := []struct {
    AmountIn *big.Int
    AmountOutMin *big.Int
    PoolAddress common.Address
    SwapType *big.Int
    TokenIn common.Address
    TokenOut common.Address
}{
    {
        AmountIn:    big.NewInt(1000000000000000000),
        AmountOutMin: big.NewInt(0),
        PoolAddress: common.HexToAddress("0x70befdbe982a2566dfd440977dba8716dcf0aa2a"),
        SwapType:    big.NewInt(1),
        TokenIn:     common.HexToAddress("0xadCae7cFec991Bd864055043439a103cbeF152f0"),
        TokenOut     common.HexToAddress("0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B"),
    },
    {
        AmountIn:    big.NewInt(650000000000000000000),
        AmountOutMin: big.NewInt(0),
        PoolAddress: common.HexToAddress("0x5d9aE87e0BE6047e99B40C3Dc0B47470710C811A"),
        SwapType:    big.NewInt(2),
        TokenIn:     common.HexToAddress("0x27be94d6fB34D09Ee7A17f6695a75bc0233EE62B"),
        TokenOut     common.HexToAddress("0xadCae7cFec991Bd864055043439a103cbeF152f0"),
    },
}
minTotalAmountOut := big.NewInt(0) // at least 0 tokens

tx, err := instance.ExecuteSwaps(&bind.TransactOpts{}, swapParams, minTotalAmountOut)
if err != nil {
    log.Fatalf("Failed to execute transaction: %v", err)
}
fmt.Printf("tx sent: %s", tx.Hash().Hex())
```

These code snippets show you how to call the functions of the `Finale` contract using go-ethereum.
