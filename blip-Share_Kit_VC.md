---
blip: <to be assigned>
title: Make Share Kit compatible with Verifiable Claims spec
author: Isaac Patka (@ipatka)
status: Draft
created: 2019-03-29
---

<!--You can leave these HTML comments in your merged BLIP and delete the visible duplicate text guides, they will not appear and may be helpful to refer to if you edit it again. This is the suggested template for new BLIPs. Note that a BLIP number will be assigned by an editor. When opening a pull request to submit your BLIP, please use an abbreviated title in the filename, `blip-draft_title_abbrev.md`. The title should be 44 characters or less.-->

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Provide a simplified and layman-accessible explanation of the BLIP.-->
Update the Share Kit payload to be compatible with the Verifiable Claims data model to improve interoperability in the decentralized identity ecosystem.

## Abstract
<!--A short (~200 word) description of the technical issue being addressed.-->
Bloom's share kit consists of an identity proof, a claim (attestation) and the associated attestation proof. Simply by updating the payload of share kit and publishing properly formated context documents share kit could be compatible with multiple decentralized identity implementations.

## Motivation
<!--The motivation is critical for BLIPs that want to change the Bloom protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the BLIP solves. BLIP submissions without sufficient motivation may be rejected outright.-->
Make Share Kit compatible with emerging decentralized identity specifications

## Specification
<!--The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Bloom platforms.-->
The below snippet is taken from the W3C Verifiable Claims data model draft specification v1.0. (https://w3c.github.io/vc-data-model/)

TODO update the example to show what Bloom's share kit would look like.

```
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  "type": "VerifiablePresentation",
  // the verifiable credential issued in the previous example
  "verifiableCredential": [{
    "id": "http://example.edu/credentials/1872",
    "type": ["VerifiableCredential", "AlumniCredential"],
    "issuer": "https://example.edu/issuers/565049",
    "issuanceDate": "2010-01-01T19:73:24Z",
    "credentialSubject": {
      "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
      "alumniOf": "<span lang='en'>Example University</span>"
    },
    "proof": {
      "type": "RsaSignature2018",
      "created": "2017-06-18T21:19:10Z",
      "creator": "https://example.edu/issuers/keys/1",
      "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..TCYt5X
        sITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUc
        X16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtj
        PAYuNzVBAh4vGHSrQyHUdBBPM"
    }
  }],
  // digital signature by Pat on the presentation establishes consent and
  // protects against replay attacks
  "proof": {
    "type": "RsaSignature2018",
    "created": "2018-09-14T21:19:10Z",
    "creator": "did:example:ebfeb1f712ebc6f1c276e12ec21#keys-1",
    // 'nonce' and 'domain' protect against replay attacks
    "nonce": "1f44d55f-f161-4938-a659-f8026467f126",
    "domain": "4jt78h47fh47",
    "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..kTCYt5
      XsITJX1CxPCT8yAV-TVIw5WEuts01mq-pQy7UJiN5mgREEMGlv50aqzpqh4Qq_PbChOMqs
      LfRoPsnsgxD-WUcX16dUOqV0G_zS245-kronKb78cPktb3rk-BuQy72IFLN25DYuNzVBAh
      4vGHSrQyHUGlcTwLtjPAnKb78"
  }
}
```

## Rationale
<!--The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->
TODO

## Backwards Compatibility
<!--All BLIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The BLIP must explain how the author proposes to deal with these incompatibilities. BLIP submissions without a sufficient backwards compatibility treatise may be rejected outright.-->
Share kit endpoints will need to be updated to support the new format. Mobile apps will probably not need to be updated since payload formatting takes place on bloom-web currently.

## Test Cases
<!--Test cases for an implementation are mandatory for BLIPs that are affecting governance changes. Other BLIPs can choose to include links to test cases if applicable.-->
TODO

## Implementation
<!--The implementations must be completed before any BLIP is given status "Final", but it need not be completed before the BLIP is accepted. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of "rough consensus and running code" is still useful when it comes to resolving many discussions of API details.-->
TODO

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/). Based on the Ethereum Improvement Proposal template.
