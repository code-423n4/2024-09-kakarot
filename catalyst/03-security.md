# Security considerations

## Notable issue classes

Due to its very nature, issues in Kakarot are more likely to have a high impact. The core contract attack surface is inevitably large, as it includes an entire VM implementation and an EVM transaction parser, necessarily exposed to malicious interactions. There are also limited applicable defense-in-depth measures, since the core contract is a central trusted component controlling all the EVM state.

This section aims to give you some hints about potential severe issues that may be found in Kakarot. While we don't suggest to limit your search to these issue classes, most of these areas have caused issues in the past and are still a source of concern.

### EVM equivalence issues

Kakarot aims for EVM equivalence -- almost any EVM contract should on Kakarot work as intended without modifications at the source (or even bytecode) level. Excluding [some known edge cases](https://docs.kakarot.org/differences/), any difference in behavior between Kakarot and geth is likely to be a valid issue. Kakarot is already tested using the Ethereum Foundation testsuite, ensuring instruction semantics match the EVM spec. Despite being testsuite-compliant, Zellic manual review revealed a few edge cases not covered by the EF testsuite (see findings 3.6 and 3.7). Additionally, Kakarot compatibility extends beyond the VM to how Ethereum transactions are parsed. Zellic manual review found several critical issues in transaction parsing.

Differential fuzzing seems a promising approach for finding differences between geth and Kakarot, we encourage you to try!

### Arithmetic issues

Cairo0 is a relatively low level language with several really unforgiving aspects. It operates on field elements (felts), which can represent values modulo [a 252 bit prime](https://docs.cairo-lang.org/how_cairo_works/cairo_intro.html#field-elements). Arithmetic operations are not checked for overflows or underflows. Since the EVM works with 256 bit integers, Kakarot emulates them by making extensive use of the Uint256 type, which represents a 256 bit number using two 128 bit felts.

The Zellic review found several issues, including:

- overflows and underflows when operating on two felts (e.g. finding 3.13)
- overflows when converting between Uint256 and felts (e.g. finding 3.10)
- unsafe Uint256 construction (using felts greater than 128 bits) (e.g. finding 3.17)

### Prover hints without appropriate Cairo constraints

One of the most fundamental concepts in zero knowledge smart contract development is the role of the prover and the verifier. In ZK environments like Starknet, smart contracts define constraints which are -- with a huge simplification -- ultimately translated to mathematical equations. A prover is responsible for providing inputs (proofs) that satisfy these equations. Verifiers maintain the network consensus by checking that the proofs provided by the provers are valid.

Cairo allows to specify [hints](https://docs.cairo-lang.org/how_cairo_works/hints.html), snippets of code that help the prover to generate proofs. Hints are not part of the smart contract, and provers are completely unaware of their existence. A prover can follow a hint, but is not required to do so. Each hint typically requires appropriate assertions to ensure the prover is not acting maliciously.

Zellic audit discovered several issues in this class (findings 3.2, 3.4, 3.8, 3.20), which were patched by the Kakarot team. They also implemented [convenient testing harnesses](https://github.com/kkrt-labs/kakarot/blob/c7896d0f23cb752f02d93743152e597d57086426/tests/utils/hints.py#L36) which allow to replace hints when running the Kakarot testsuite. We encourage you to experiment with it, you may find other similar issues! 

### Data packing/unpacking, RLP decoding, and transaction parsing

Kakarot typically stores bytestreams as an array of felts, efficiently packing multiple bytes (up to 31) into each felt. This includes the bytecode of a contract, or the bytestream of an incoming transaction. When needed, packed bytes felt arrays are unpacked to a longer array of felts where each element represents a single byte.

Once unpacked, transaction data needs to be decoded from [Recursive Length Prefix format (RLP)](https://ethereum.org/en/developers/docs/data-structures-and-encoding/rlp/). The Kakarot RLP decoder and the packing/unpacking routines were sources of multiple issues (including 3.2, 3.3, 3.4, 3.12), especially related to length checks. The most common root causes of high severity issues were integer overflows and the lack of input length checks leading to out of bound reads/TOCTOUs.

Once the input bytestream is decoded into a stream of RLP items, their structure needs to be validated to ensure it represents a valid Ethereum transaction. This is another possible source of issues worth focusing on (ref. Zellic issues 3.5 and 3.16).

