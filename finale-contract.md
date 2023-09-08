---
sidebar_position: 1
---

# Finale Contract Documentation

## Introduction

The `FinaleFinance` contract is a Solidity smart contract designed for swapping tokens. It interacts with two types of routers: `syncRouter` and `muteRouter`. The contract contains functions to approve and revoke maximum token allowances for these routers. It also includes functions to perform swaps on these routers and execute multiple swaps in sequence.

## Contract Inheritance

The Finale contract inherits from ReentrancyGuard, ContractErrors, and Ownable.

- ReentrancyGuard: Provides a modifier to prevent reentrant calls to a function.
- ContractErrors: Provides error messages that can be used throughout the contract.
- Ownable: Provides a system for transferring ownership of the contract and restricting access to certain functions to the contract's owner.

The contract also uses the Math library for performing arithmetic operations.

## State Variables

- ``syncRouter``: Instance of the ISyncRouter contract interface.
- ``_syncrouterAddress``: The address of the syncRouter contract.
- ``muteRouter``: Instance of the IMuteRouter contract interface.
- ``_muteRouterAddress``: The address of the muteRouter contract.
- ``_fee_address``: The address where the fee is sent after executing swaps.

## Events

- ``SwapExecuted``: Emitted when a swap operation is completed. It includes details about the swap, such as the user who performed the swap, the input and output tokens, and the type of swap (sync or mute).
- ``PathsExecuted``: Emitted when a sequence of swaps is executed. It includes details about the swaps, such as the user who performed the swaps, the parameters of each swap, the minimum total amount of output tokens expected, and the final amount of output tokens received.

## Constructor

The constructor initializes the syncRouter and muteRouter contract instances using the addresses stored in _syncrouterAddress and _muteRouterAddress respectively.

## Function Descriptions

``maxApprovals``: This function is used to approve the maximum possible amount of tokens for the syncRouter and muteRouter. It takes an array of token addresses as input and iterates through the array, approving the maximum allowance for each token. Only the owner of the contract can call this function.

``revokeApprovals``: This function is used to revoke the token approvals granted to the syncRouter and muteRouter. It takes an array of token addresses as input and iterates through the array, setting the allowance for each token to zero. Only the owner of the contract can call this function.

``syncswap``: This function is used to perform a swap operation on the syncRouter. It takes as input the address of the pool to perform the swap in, the address of the input token, and the minimum amount of output tokens expected from the swap. It returns the amount and type of output tokens received from the swap.

``muteswap``: This function is used to perform a swap operation on the muteRouter. It takes as input the addresses of the input and output tokens, and the minimum amount of output tokens expected from the swap. It returns the amount and type of output tokens received from the swap.

``executeSwaps``: This function is used to execute a sequence of swap operations. It takes as input an array of SwapParam structures, each of which specifies the parameters for a swap operation, and a minimum total amount of output tokens expected from all the swaps. If the total amount of output tokens received from the swaps is less than the minimum expected amount, the function reverts. After executing the swaps, the function transfers a fee (0.03% of the final amount of output tokens) to the _fee_address and sends the remaining output tokens to the user who called the function.

## Error Handling

The contract uses revert statements with error messages defined in the ContractErrors contract to handle errors. It checks for errors such as failure to transfer tokens, failure to approve tokens, and receiving less output tokens than expected from swaps.