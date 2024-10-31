# Preamble

SIP Number: 029

Title: Bootstrapping sBTC Liquidity and Nakamoto Signer Incentives

Authors:

- Brittany Laughlin (brittany@stacks.org)
- Jesse Wiley (jesse@stacks.org)
- Jude Nelson (jude@stacks.org)
- Will Corcoran (will@stacks.org)

Consideration: Economics, Technical

Type: Consensus (hard fork)

Status: Draft

Created: 2024-10-31

License: BSD 2-Clause

Sign-off:

Discussions-To: https://forum.stacks.org/t/aligning-with-bitcoin-halving-and-incentives-after-nakamoto/17668

# Abstract

This SIP proposes modifying the Stacks token emission schedule, with the intent of preserving critical network incentives during the sBTC launch and the Nakamoto upgrade. This proposal would extend the current emission schedule until April 2026, aligning subsequent halvings with Bitcoin while maintaining a 100 STX tail emission and resulting in a reduced 2050 supply, which is slightly below the originally calculated 1.818B STX[1].

# Introduction

1. sBTC’s success hinges on its liquidity. Delaying the current halvening schedule helps maintain steady PoX rewards through its initial launch, ensuring that early sBTC adopters receive a BTC yield which attracts both liquidity and developer talent. [2]

2. Nakamoto signers validate, sequence, and sign Stacks blocks in exchange for PoX rewards. To avoid disrupting the incentive structure shortly after Nakamoto’s launch, a predictable and stable PoX yield is necessary to retain high-quality participation.

3. As a side effect of this proposal, Stacks’ halvening schedule would align with Bitcoin's and serve to strengthen the Bitcoin-Stacks connection by providing a more predictable token emission schedule for both miners and users. [2]


# Specification

This proposal intends to replace the existing Stacks token emission scheduled in the following way:

## Proposed STX Coinbase Schedule

| Coinbase Reward Reduction Phase | Approximate Date (time b/w halvings) | STX Reward (reduction) | STX Supply (after) |
|---------------------------------|--------------------------------------|------------------------|--------------------|
| Current                         | -                                    | 1000                   | -                  |
| 1st*                            | ~April 2026 (+1.33 yrs)              | 500 (50%)              | 1,607,907,038      |
| 2nd                             | ~April 2028 (2 yrs)                  | 250 (50%)              | 1,652,242,188      |
| 3rd                             | ~April 2032 (4 yrs)                  | 125 (50%)              | 1,696,547,013      |
| 4th                             | ~April 2036 (4 yrs)                  | 100 (20%)              | 1,718,699,425      |
| -                               | ~Jan 2050 (13.833 yrs)               | 100 (0%)               | 1,779,628,415      |


## Current STX Coinbase Schedule
*Note: Provided for reference*

| Coinbase Reward Reduction Phase | Approximate Date (time b/w halvings) | STX Reward (reduction) | STX Supply (after) |
|---------------------------------|--------------------------------------|------------------------|--------------------|
| Current                         | -                                    | 1000                   | -                  |
| 1st                             | ~Dec 2024 (4 yrs)                    | 500 (50%)              | 1,512,993,315      |
| 2nd                             | ~Dec 2028 (4 yrs)                    | 250 (50%)              | 1,645,084,888      |
| 3rd                             | ~Dec 2032 (4 yrs)                    | 125 (50%)              | 1,689,389,675      |
| -                               | ~Jan 2050 (17.08 yrs)                | 125 (0%)               | 1,783,063,600      |

## Supply Impact

If accepted, the projected STX supply in January 2050 will be `1,779,628,415` STX, which is:

- Less than current projection of `1,783,063,600` STX
- Below the initial projected 2050 supply cap of `1,818B` STX (*0.19%*)

# Backwards Compatibility

This change requires a hard fork of the Stacks blockchain. All nodes must upgrade to maintain consensus.

# Activation
NOTE: copied from sip-021

[Voting addresses and technical details to be added]


## Voting Timeline

Voting will occur during reward cycle 97 (November 11-26, 2024).

## Process of Activation

There are different rules for activating this SIP based on whether or not the user has stacked their STX, and how they have done so.

### For Stackers

In order for this SIP to activate, the following criteria must be met by the set of Stacked STX:
- At least double the amount of Stacked STX locked by the largest Stacker in the cycle preceding the vote must vote at all to activate this SIP.
- Of the Stacked STX that vote, at least 80% of them must vote "yes."

The act of not voting is the act of siding with the outcome, whatever it may be. We believe that these thresholds are sufficient to demonstrate interest from Stackers -- Stacks users who have a long-term interest in the Stacks blockchain's successful operation -- in performing this upgrade.

### How To Vote
If a user is Stacking, then their STX can be used to vote in one of two ways, depending on whether or not they are solo-stacking or stacking through a delegate.

The user must be Stacking in any cycle up to and including a cycle to be determined that is no later than cycle 81. Their vote contribution will be the number of STX they have locked.

#### Solo Stacking

The user must send a minimal amount of BTC from their PoX reward address to one of the following Bitcoin addresses:

- For **"yes"**, the address is `11111111111111X6zHB1bPW6NJxw6`. This is the base58check encoding of the hash in the Bitcoin script `OP_DUP` `OP_HASH160` `000000000000000000000000007965732d332e30` `OP_EQUALVERIFY` `OP_CHECKSIG`. The value `000000000000000000000000007965732d332e30` encodes "yes-3.0" in ASCII, with 0-padding.

For **"no"**, the address is `1111111111111117Crbcbt8W5dSU7`. This is the base58check encoding of the hash in the Bitcoin script OP_DUP OP_HASH160 `00000000000000000000000000006e6f2d332e30` `OP_EQUALVERIFY` `OP_CHECKSIG`. The value `00000000000000000000000000006e6f2d332e30` encodes "no-3.0" in ASCII, with 0-padding.

From there, the vote tabulation software will track the Bitcoin transaction back to the PoX address in the .pox-3 contract that sent it, and identify the quantity of STX it represents. The STX will count towards a "yes" or "no" based on the Bitcoin address to which the PoX address sends.

If the PoX address holder votes for both "yes" and "no" by the end of the vote, the vote will be discarded.

Note that this voting procedure does not apply to Stacking pool operators. Stacking pool operator votes will not be considered.

#### Pooled Stacking

If the user is stacking in a pool, then they must send a minimal amount of STX from their Stacking address to one of the following Stacks addresses to commit their STX to a vote:

- For **"yes"**, the address is `SP00000000000003SCNSJTCSE62ZF4MSE`. This is the c32check-encoded Bitcoin address for "yes" (`11111111111111X6zHB1bPW6NJxw6`) above.
- For **"no"**, the address is `SP00000000000000DSQJTCSE63RMXHDP`. This is the c32check-encoded Bitcoin address for "no" (`1111111111111117Crbcbt8W5dSU7`) above.

From there, the vote tabulation software will track the STX back to the sender, and verify that the sender also has STX stacked in a pool. The Stacked STX will be tabulated as a "yes" or "no" depending on which of the above two addresses receive a minimal amount of STX.

If the Stacks address holder votes for both "yes" and "no" by the end of the vote period, the vote will be discarded.

## For Non-Stackers

Users with liquid STX can vote on proposals using the [Ecosystem DAO](https://stx.eco).
Liquid STX is the users balance, less any STX they have locked in PoX stacking protocol,
at the block height at which the voting started (preventing the same STX from being transferred between accounts and used to effectively double vote).
This is referred to generally as "snapshot" voting.

For SIP 21 Nakamoto Upgrade to pass 66% of all liquid STX committed by voting
must in favour of the proposal.

### For Miners
There is only one criterion for miners to activate this SIP: they must mine the Stacks blockchain up to and past the end of the voting period. In all reward cycles between cycle 75 and the end of the voting period, PoX must activate.

### Examples

#### Voting "yes" as a solo Stacker

Suppose Alice has stacked 100,000 STX to `1LP3pniXxjSMqyLmrKHpdmoYfsDvwMMSxJ` during at least one of the voting period's reward cycles. To vote, she sends 5500 satoshis for **yes** to `11111111111111X6zHB1bPW6NJxw6`. Then, her 100,000 STX are tabulated as "yes".

#### Voting "no" as a pool Stacker

Suppose Bob has Stacked 1,000 STX in a Stacking pool and wants to vote "no", and suppose it remains locked in PoX during at least one reward cycle in the voting period. Suppose his Stacks address is `SP2REA2WBSD3XMVMYS48NJKS3WB22JTQNB101XRRZ`. To vote, he sends 1 uSTX from `SP2REA2WBSD3XMVMYS48NJKS3WB22JTQNB101XRRZ` for no to `SP00000000000000DSQJTCSE63RMXHDP`. Then, his 1,000 STX are tabulated as "no."


# Reference Implementation

[To be added: Link to implementation PR]

NOTE: md code block for this function: https://github.com/stacks-network/stacks-core/blob/master/stackslib/src/chainstate/stacks/db/blocks.rs#L3614-L3650


## Appendix
NOTE: consider copying the reports locally in the PR (particularly the spreadsheet, which would be better suited as a pdf)

[1] "Mining Emissions and Risks of the STX Halving", 7th Avenue Group, July 2023. Available at https://stx.is/emissions-report-1 [Verified 31 October 2024]

[2] "Halving Proposals", 7th Avenue Group, November 2023. Available at https://stx.is/emissions-report-2 [Verified 31 October 2024]

[3] "STX Halving Model", 7th Avenue Group, November 2023. Available at https://docs.google.com/spreadsheets/d/1DiUv-Z8fENKT2gTniYX0DrNyQYl8ZZTsvj-ZhR9S75g/edit?gid=1943414414#gid=1943414414 [Verified 31 October 2024]


