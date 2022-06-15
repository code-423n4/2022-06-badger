# Badger-Vested-Aura contest details
- $28,500 USDC main award pot
- $1,500 USDC gas optimization award pot
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-06-badger-vested-aura-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts June 15, 2022 20:00 UTC
- Ends June 18, 2022 19:59 UTC


[ ⭐️ SPONSORS ADD INFO HERE ]

# Badger Vested Aura - bveAURA

Building upon the work of the CodeArena Reviewed bveCVX (Badger Vested CVX), Badger is offering a locking vault for Aura.finance, with the goal of sourcing yield from locking incetives as well as processing voting bribes from HiddenHands.

## Code Repository

https://github.com/Badger-Finance/vested-aura/tree/c-1

The repository is a brownie project, just setup a .env with your `WEB3_INFURA_PROJECT_ID` and `ETHERSCAN_TOKEN`

```
# .env

WEB3_INFURA_PROJECT_ID=
ETHERSCAN_TOKEN=
```


Run the tests with

`
brownie test --interactive
`

`--interactive` allows you to debug if something breaks

Check for gas and coverage with
`
brownie test --gas --coverage
`


## Badger Basic Architecture

bveAura, is built with Badger Vaults 1.5, a Quantstamp Audited improvement over Yearn Vaults V1

The Vault is meant to manage Deposits and Withdrawals, while the Strategy is meant to lock AURA and use the lock to claim locking rewards as well as Bribes from Hidden Hands.

The Claimed Bribes are transfered to another smart contract, out of scope, called the BribesProcessor which is meant to be used by a multi-sig to sell the bribes into other tokens.

## Important Considerations

Because of the architecture of BadgerVaults, not only the Strategy contract could contain vulnerabilities, but vulnerabilities could emerge from the interaction between the Strategy, the BaseStrategy and the Vault.

Particular care should be put into understanding how Vault invariant could be broken due to the implementation of the `MyStrategy`

## Contracts in Scope

- MyStrategy.sol - 440 LOC -> https://github.com/Badger-Finance/vested-aura/blob/c-1/contracts/MyStrategy.sol

## Contracts that can be brough into scope for High and Med Severity findings

The following contracts are part of the architecture, they are already audited, have been reviewed already in the previous CodeArena Contest.
The following Contracts can be brought into scope for High and Medium severity finding, however we will not accept Gas nor QA reports

- Vault.sol -> https://github.com/Badger-Finance/badger-vaults-1.5/blob/0.1.0/contracts/Vault.sol
- BaseStrategy.sol -> https://github.com/Badger-Finance/badger-vaults-1.5/blob/0.1.0/contracts/BaseStrategy.sol

## Notes about certain findings

Because of the system integrating with already audited code, the contracts not explicitly mentioned in scope can be brough into scope for Medium and High Severity findings, however we will not be accepting gas optimization nor refactorings for code not in scope.

E.g. If you can find a vulnerability in the interaction between Vault and Strategy, the please do disclose the vulnerability, however do not send us gas optimizations for Vault.sol as the contract is not in scope.

## Request for Gas Findings

Having reviewed hundreds of gas reports I ask wardens to kindly report Gas Findings along with an estimate of how much gas it would save.

Adding the gas estimate forces you into figuring that out properly, allows a easier review and helps you win more contests.

e.g. `Cache Storage Variable - 97 gas`

## Attack Ideas

Wardens should try to:

- Break the vault accounting by inflating the value of the underlying vs the real value redeemable (socialize losses)
- Have the vault be denominated into a token which price can be manipulated (single sided exposure)
- Loss of Yield
- Loss of Bribes
- Loss of Voting Power
- Inability to Withdraw due to incorrect logic
- Griefing attacks (force to relock when not expected)
- Donation attack to rebase token and break invariants

## Additional Resources

bveCVX CodeArena Report

https://code4rena.com/reports/2021-09-bvecvx

Badger Vaults 1.5 Codebase

https://github.com/Badger-Finance/badger-vaults-1.5

b1.5 Code Overview: https://www.youtube.com/watch?v=u__v-J7KTNM

b1.5 Documentation: https://docs.badger.com/badger-finance/wip-vaults-1.5

Badger Vaults 1.5 QuantStamp Audit
https://github.com/Badger-Finance/badger-vaults-1.5/blob/main/security/audits/Badger%20Vaults%201.5%20-%20Quantstamp%20-%20Jan%202022.pdf

Badger Office Hour Discussion about the Why behind 1.5:
https://www.youtube.com/watch?v=psf4BS_kPIc&ab_channel=BadgerDAO
