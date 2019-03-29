---
blip: <to be assigned>
title: Make BloomID DID Compatible
author: Isaac Patka (@ipatka)
status: Draft
created: 2019-03-29
---

<!--You can leave these HTML comments in your merged BLIP and delete the visible duplicate text guides, they will not appear and may be helpful to refer to if you edit it again. This is the suggested template for new BLIPs. Note that a BLIP number will be assigned by an editor. When opening a pull request to submit your BLIP, please use an abbreviated title in the filename, `blip-draft_title_abbrev.md`. The title should be 44 characters or less.-->

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Provide a simplified and layman-accessible explanation of the BLIP.-->
Make the BloomID mobile app DID compatible to improve interoperability.

## Abstract
<!--A short (~200 word) description of the technical issue being addressed.-->
Bloom uses Ethereum keypairs to identify unique BloomID users. Users currently sign up for Bloom by downloading the iOS app, Android app or via Metamask on web. Bloom should add DIDs to user profiles which resolve to documents explaining the BloomID schema.

## Motivation
<!--The motivation is critical for BLIPs that want to change the Bloom protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the BLIP solves. BLIP submissions without sufficient motivation may be rejected outright.-->
This change will improve Bloom's interoperability within the decentralized identity ecosystem.

## Specification
<!--The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Bloom platforms.-->
The below example is taken from the W3C DID Data Model Draft v0.12 (https://w3c-ccg.github.io/did-spec/)

TODO update this with example Bloom DID document.

```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:example:123456789abcdefghi",
  "authentication": [{
    // this key can be used to authenticate as did:...fghi
    "id": "did:example:123456789abcdefghi#keys-1",
    "type": "RsaVerificationKey2018",
    "controller": "did:example:123456789abcdefghi",
    "publicKeyPem": "-----BEGIN PUBLIC KEY...END PUBLIC KEY-----\r\n"
  }],
  "service": [{
    "type": "ExampleService",
    "serviceEndpoint": "https://example.com/endpoint/8377464"
  }]
}
```


## Rationale
<!--The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->
TODO

## Backwards Compatibility
<!--All BLIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The BLIP must explain how the author proposes to deal with these incompatibilities. BLIP submissions without a sufficient backwards compatibility treatise may be rejected outright.-->
Within the Bloom ecosystem no changes will be needed because Bloom endpoints already know how to interpret BloomIDs. Once this change is made users will likely need to update their mobile app in order to interface with other DID compliant services.

## Test Cases
<!--Test cases for an implementation are mandatory for BLIPs that are affecting governance changes. Other BLIPs can choose to include links to test cases if applicable.-->
TODO

## Implementation
<!--The implementations must be completed before any BLIP is given status "Final", but it need not be completed before the BLIP is accepted. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of "rough consensus and running code" is still useful when it comes to resolving many discussions of API details.-->
TODO

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/). Based on the Ethereum Improvement Proposal template.
