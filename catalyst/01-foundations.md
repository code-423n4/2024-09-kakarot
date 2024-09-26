# Foundational knowledge

## Kakarot zkEVM 

Kakarot is a zkEVM written in [Cairo0](https://docs.cairo-lang.org/hello_cairo/index.html). The project is aiming for deployment on Starknet Mainnet in Q3 2024. However, it is written to be largely agnostic of Starknet-specific characteristics, and should be easily adapted to allow any CairoVM-based ecosystem to run EVM contracts with minimal to no modifications to existing contracts. The design also allows interoperability between contracts running in the zkEVM and native contracts running in the host CairoVM.

## Fundamentals and recommended background

Recommended prerequisites for effectively auditing Kakarot include:

### Cairo0
**Cairo0**: not to be confused with the more modern, rust-looking Cairo1, Cairo0 is the language used to develop Kakarot. 

It is a relatively low-level language, and due to the atypical constraints posed by the zero-knowledge VM environment in which it runs (the CairoVM), it has several design choices that will be surprising to the uninitiated. Documentation about the language can be found [at this link](https://docs.cairo-lang.org/hello_cairo/index.html). 

At times, you may need to reference the standard library souce code, which can be found [in the starkware-libs/cairo-lang](https://github.com/starkware-libs/cairo-lang/tree/master/src/starkware/cairo/common) Github repo. You may find some concepts to be explained more clearly in the [Cairo1 documentation](https://book.cairo-lang.org/ch00-00-introduction.html). Be careful, however, as Cairo1 is significantly different from Cairo0.

### Starknet

**Starknet**: some aspects of Kakarot are tied to Starknet implementation details. Useful readings include documentation about the [transaction lifecycle](https://docs.starknet.io/architecture-and-concepts/network-architecture/transaction-life-cycle/), [the account abstraction interface](https://docs.starknet.io/architecture-and-concepts/accounts/account-functions/), and about [system calls](https://docs.starknet.io/architecture-and-concepts/smart-contracts/system-calls-cairo1/).

### EVM

**EVM**: An in-depth understanding of the EVM design is a must. Vast resources are available, including:
- [the Ethereum Yellowpaper](https://ethereum.github.io/yellowpaper/paper.pdf), which originally defined the EVM
- [evm.codes](https://www.evm.codes/), a handy reference for looking up instruction semantics
- the [go-ethereum](https://github.com/ethereum/go-ethereum/tree/master/core/vm) and [py-evm](https://github.com/ethereum/py-evm/tree/main/eth/vm) implementations, officially sanctioned by the Ethereum Foundation