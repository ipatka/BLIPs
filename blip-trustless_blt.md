---
blip: <to be assigned>
title: Trustless BLT ERC20 Token
author: Isaac Patka (@ipatka)
status: Draft
created: 2018-11-28
---

<!--You can leave these HTML comments in your merged BLIP and delete the visible duplicate text guides, they will not appear and may be helpful to refer to if you edit it again. This is the suggested template for new BLIPs. Note that a BLIP number will be assigned by an editor. When opening a pull request to submit your BLIP, please use an abbreviated title in the filename, `blip-draft_title_abbrev.md`. The title should be 44 characters or less.-->

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Provide a simplified and layman-accessible explanation of the BLIP.-->
Remove privileged keys from BLT contract. Bloom currently holds keys which enable the holder to have administrative control over some token functionality. These privileges should be revoked by relinquishing ownership.

## Abstract
<!--A short (~200 word) description of the technical issue being addressed.-->
Remove privileged keys from BLT contract. Bloom currently holds keys which enable the holder to have administrative control over some token functionality. These privileges include granting vested tokens, minting tokens, burning tokens, freeze transfers. These privileges are commonly found in dApps with ERC20 token as they allow project creators to respond to discovered vulnerabilites. However in order to be a truly decentralized protocol these privileges must be revoked. This BLIP proposes a procedure to revoke admin privileges from the Bloom Company over the BLT ERC20 token.

TODO what are all the keys that need to be burned?

## Motivation
<!--The motivation is critical for BLIPs that want to change the Bloom protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the BLIP solves. BLIP submissions without sufficient motivation may be rejected outright.-->
Make Bloom's token trustless and decentralized in support of the company's mission.

## Specification
<!--The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Bloom platforms.-->

### Minting

### Burning

### Freezing

### Granting vested tokens

## Rationale
<!--The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->
TODO

## Backwards Compatibility
<!--All BLIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The BLIP must explain how the author proposes to deal with these incompatibilities. BLIP submissions without a sufficient backwards compatibility treatise may be rejected outright.-->
This change is fully backward compatible with all third parties that interface with the BLT token. The contract address and ERC20 function interfaces are not changing.

## Test Cases
<!--Test cases for an implementation are mandatory for BLIPs that are affecting governance changes. Other BLIPs can choose to include links to test cases if applicable.-->
No core ERC20 features are affected.
TODO

## Implementation
<!--The implementations must be completed before any BLIP is given status "Final", but it need not be completed before the BLIP is accepted. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of "rough consensus and running code" is still useful when it comes to resolving many discussions of API details.-->
TODO

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/). Based on the Ethereum Improvement Proposal template.
