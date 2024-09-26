# Kakarot zkEVM Audit Catalyst

## Purpose

This audit catalyst was prepared by Zellic security researchers to accelerate competitive auditors' efficiency and effectiveness in contributing to the security of Kakarot in anticipation of the September–October 2024 Code4rena competitive audit.

## Outline

- [Foundational knowledge](01-foundations.md)
- Kakarot zkEVM
- Fundamentals and recommended background
  - Cairo0
  - Starknet
  - EVM
- [Project overview](02-overview.md)
  - Core contract
  - Account contract
  - Account contract deployment
  - Transaction flow
- [Security considerations](03-security.md)
- Notable issue classes
  - EVM equivalence issues
  - Arithmetic issues
  - Prover hints without appropriate Cairo constraints
  - Data packing/unpacking, RLP decoding, and transaction parsing