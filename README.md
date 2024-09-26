# Kakarot audit details
- Total Prize Pool: $170,000 in USDC
  - HM awards: $93,400 in USDC
  - Z Pool (Zenith side pool): $45,000 in USDC
  - [Audit Catalyst](https://discord.com/channels/810916927919620096/810929015509483554/1286822018334982155) awards: $12,500 in USDC
  - Judge awards: $11,200 in USDC
  - Validator awards: $7,400 in USDC
  - Scout awards: $500 in USDC
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts September 27, 2024 20:00 UTC
- Ends October 25, 2024 20:00 UTC

**Z Pool and Dark Horse Bonus Pool**
- This audit includes two [Zenith](https://code4rena.com/zenith) Researchers (ZRs), who are designated as leads for the audit ("LZRs").
- Dark Horse wardens earn a portion of the Z pool by outperforming (or tying) the top-ranked LZR auditor based on [Gatherer score](https://docs.code4rena.com/awarding/incentive-model-and-awards#bonuses-for-top-competitors). 
- For more details, see [Z Pool / Dark Horse bonus pool distribution rules](https://docs.code4rena.com/roles/certified-contributors)

ℹ️ While there are no QA awards, QA reports are encouraged as a fallback in the event of no valid HMs. 

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-09-kakarot/blob/main/4naly3er-report.md).

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

• Front-end components
• Infrastructure relating to the project
• Key custody
• The owner of Kakarot has total admin rights to update classes and write values to account storages.
• EVM Gas not being equivalent to Starknet opcode pricing

Any documented difference between Kakarot and Ethereum in the docs https://docs.kakarot.org/differences

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

# Overview

[ ⭐️ SPONSORS: add info here ]

## Audit Catalyst

This project includes an [Audit Catalyst](catalyst/README.md) prepared by Zellic security researchers which aims to be an essential read for accelerating your work as an auditor in using your time most effectively in contributing to the security of the project.

## Links

- **Previous audits:**   https://github.com/kkrt-labs/kakarot/blob/main/audits/Kakarot%20EVM%20-%20Zellic%20Audit%20Report.pdf
  - ✅ SCOUTS: If there are multiple report links, please format them in a list.
- **Documentation:** https://docs.kakarot.org/
- **Website:** https://www.kakarot.org/
- **X/Twitter:** https://x.com/KakarotZkEvm
- **Discord:** https://discord.com/invite/kakarotzkevm

---

# Scope

[ ✅ SCOUTS: add scoping and technical details here ]

### Files in scope
- ✅ This should be completed using the `metrics.md` file
- ✅ Last row of the table should be Total: SLOC
- ✅ SCOUTS: Have the sponsor review and and confirm in text the details in the section titled "Scoping Q amp; A"

*For sponsors that don't use the scoping tool: list all files in scope in the table below (along with hyperlinks) -- and feel free to add notes to emphasize areas of focus.*

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [contracts/folder/sample.sol](https://github.com/code-423n4/repo-name/blob/contracts/folder/sample.sol) | 123 | This contract does XYZ | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |


> SCOUTS: Hi, Sock here! 💜 
> Just helping get this set up. Please massage however needed.
> 
> Note that there's some interestingness here. Quoting eni from Kakarot:
> 
> > crates/evm/src/precompiles/sha256.cairo imports an item from crates/utils/src/math.cairo - Should I consider that math.cairo this file also be in the list, or are any items imported in the included files implicitly in scope? 
> > 
> > In our case this is a bit special because these files can contain shared code that is not used in what we want included in scope. 
> > 
> > For context:
> > - Kakarot is the CairoZero implementation
> > - KakarotSSJ is the Cairo (new) implementation, out of scope
> > - Because Starknet has introduced new features in new Cairo, we have Cairo classes called by CairoZero contracts, notably for EVM precompile execution
> > - These classes are based on code defined in the KakarotSSJ repository, and called through code defined in the Kakarot repository.

> My advice was:
> 
> > I would highly recommend is to create a detailed readme that specifically enumerates the files and, in these cases, the functions that you would like to include in scope of the auditors’ attention 
> 
> So these are based on that. Will defer to your advice on how best to include the specific functions for focus in `kakarot-ssj`

### kakarot 697100af34444b3931c18596cec56c454caf28ed

```
src/utils/utils.cairo
src/utils/eth_transaction.cairo
src/utils/bytes.cairo
src/utils/maths.cairo
src/utils/dict.cairo
src/utils/signature.cairo
src/utils/rlp.cairo
src/utils/uint256.cairo
src/utils/array.cairo
src/backend/starknet.cairo
src/kakarot/storages.cairo
src/kakarot/instructions/push_operations.cairo
src/kakarot/instructions/stop_and_math_operations.cairo
src/kakarot/instructions/environmental_information.cairo
src/kakarot/instructions/sha3.cairo
src/kakarot/instructions/logging_operations.cairo
src/kakarot/instructions/block_information.cairo
src/kakarot/instructions/duplication_operations.cairo
src/kakarot/instructions/memory_operations.cairo
src/kakarot/instructions/system_operations.cairo
src/kakarot/instructions/exchange_operations.cairo
src/kakarot/account.cairo
src/kakarot/gas.cairo
src/kakarot/errors.cairo
src/kakarot/kakarot.cairo
src/kakarot/interpreter.cairo
src/kakarot/precompiles/blake2f.cairo
src/kakarot/precompiles/ripemd160.cairo
src/kakarot/precompiles/precompiles.cairo
src/kakarot/precompiles/precompiles_helpers.cairo
src/kakarot/precompiles/kakarot_precompiles.cairo
src/kakarot/precompiles/ec_recover.cairo
src/kakarot/precompiles/p256verify.cairo
src/kakarot/precompiles/sha256.cairo
src/kakarot/precompiles/identity.cairo
src/kakarot/events.cairo
src/kakarot/memory.cairo
src/kakarot/accounts/uninitialized_account.cairo
src/kakarot/accounts/account_contract.cairo
src/kakarot/accounts/library.cairo
src/kakarot/accounts/model.cairo
src/kakarot/eth_rpc.cairo
src/kakarot/state.cairo
src/kakarot/evm.cairo
src/kakarot/library.cairo
src/kakarot/constants.cairo
src/kakarot/model.cairo
src/kakarot/interfaces/interfaces.cairo
src/kakarot/stack.cairo
```

### kakarot-lib c2c7cb400f85c3699a6902946bcf4428d5b4fc61

```
src/CairoLib.sol
```

### kakarot-ssj d4a7873d6f071813165ca7c7adb2f029287d14ca

```
crates/contracts/src/cairo1_helpers.cairo
crates/evm/src/errors.cairo
crates/evm/src/precompiles/es_operations/ec_add.cairo
crates/evm/src/precompiles/ec_operations/ec_mul.cairo
crates/evm/src/precompiles/ec_operations.cairo
crates/evm/src/precompiles/sha256.cairo
crates/utils/src/helpers.cairo -> `fn load_word`
crates/utils/src/traits/bytes.cairo -> `trait ToBytes<T>`, `trait FromBytes`, `fn pad_right_with_zeroes`
crates/utils/src/math.cairo -> `trait Bitshift`, `trait Exponentiation`
```

### Files out of scope
✅ SCOUTS: List files/directories out of scope

## Scoping Q &amp; A

### General questions
### Are there any ERC20's in scope?: Yes

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".

Specific tokens (please specify)
Starknet's ETH token - an ERC20 defined in https://github.com/starknet-io/starkgate-contracts

### Are there any ERC777's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



### Are there any ERC721's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



### Are there any ERC1155's in scope?: No

✅ SCOUTS: If the answer above 👆 is "Yes", please add the tokens below 👇 to the table. Otherwise, update the column with "None".



✅ SCOUTS: Once done populating the table below, please remove all the Q/A data above.

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       🖊️             |
| Test coverage                           | ✅ SCOUTS: Please populate this after running the test coverage command                          |
| ERC721 used  by the protocol            |            🖊️              |
| ERC777 used by the protocol             |           🖊️                |
| ERC1155 used by the protocol            |              🖊️            |
| Chains the protocol will be deployed on | OtherStarknet  |

### ERC20 token behaviors in scope

| Question                                                                                                                                                   | Answer |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| [Missing return values](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#missing-return-values)                                                      |   In scope  |
| [Fee on transfer](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#fee-on-transfer)                                                                  |  Out of scope  |
| [Balance changes outside of transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#balance-modifications-outside-of-transfers-rebasingairdrops) | Out of scope    |
| [Upgradeability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#upgradable-tokens)                                                                 |   In scope  |
| [Flash minting](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#flash-mintable-tokens)                                                              | Out of scope    |
| [Pausability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#pausable-tokens)                                                                      | In scope    |
| [Approval race protections](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#approval-race-protections)                                              | Out of scope    |
| [Revert on approval to zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-approval-to-zero-address)                            | In scope    |
| [Revert on zero value approvals](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-approvals)                                    | In scope    |
| [Revert on zero value transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                    | In scope    |
| [Revert on transfer to the zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-transfer-to-the-zero-address)                    | In scope    |
| [Revert on large approvals and/or transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-large-approvals--transfers)                  | In scope    |
| [Doesn't revert on failure](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#no-revert-on-failure)                                                   |  In scope   |
| [Multiple token addresses](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                          | Out of scope    |
| [Low decimals ( < 6)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#low-decimals)                                                                 |   Out of scope  |
| [High decimals ( > 18)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#high-decimals)                                                              | Out of scope    |
| [Blocklists](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#tokens-with-blocklists)                                                                | In scope    |

### External integrations (e.g., Uniswap) behavior in scope:


| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | No   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   No  |


### EIP compliance checklist
N/A

✅ SCOUTS: Please format the response above 👆 using the template below👇

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| src/Token.sol                           | ERC20, ERC721                |
| src/NFT.sol                             | ERC721                       |


# Additional context

## Main invariants

- Transactions can only be submitted to Kakarot by the Starknet account corresponding to an EVM account

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## Attack ideas (where to focus for bugs)
• Underconstrained computations
• Overflows / Underflows
• Missing range checks
• DoS attacks

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## All trusted roles in the protocol

- Owner of the Kakarot Contract

✅ SCOUTS: Please format the response above 👆 using the template below👇

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| Owner                          | Has superpowers                |
| Administrator                             | Can change fees                       |

## Describe any novel or unique curve logic or mathematical models implemented in the contracts:

N/A

✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

## Running tests

Follow instructions in the README.md and CONTRIBUTING.md guide for environment setup.

✅ SCOUTS: Please format the response above 👆 using the template below👇

```bash
git clone https://github.com/code-423n4/2023-08-arbitrum
git submodule update --init --recursive
cd governance
foundryup
make install
make build
make sc-election-test
```
To run code coverage
```bash
make coverage
```
To run gas benchmarks
```bash
make gas
```

✅ SCOUTS: Add a screenshot of your terminal showing the gas report
✅ SCOUTS: Add a screenshot of your terminal showing the test coverage

## Miscellaneous
Employees of Kakarot and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.
