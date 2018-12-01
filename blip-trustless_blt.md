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
Remove privileged keys from BLT contract. Bloom currently holds keys which enable the holder to have administrative control over some token functionality. These privileges include granting vested tokens, minting tokens, burning tokens and freezing transfers. These privileges are commonly found in dApps with ERC20 token as they allow project creators to respond to discovered vulnerabilites. However in order to be a truly decentralized protocol these privileges must be revoked. This BLIP proposes a procedure to revoke admin privileges from the Bloom Company over the BLT ERC20 token.

The `Token Controller` may not be able to be self destructed or burned by transferring control to the `0x0` address. This is becase the `Token Controller` is a smart contract that implements certain methods which are called during all `BLT` actions like transfers. If the controller is disabled improperly it will freeze all ERC20 functionality.


## Motivation
<!--The motivation is critical for BLIPs that want to change the Bloom protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the BLIP solves. BLIP submissions without sufficient motivation may be rejected outright.-->
Make Bloom's token trustless and decentralized in support of the company's mission.

## Specification
<!--The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Bloom platforms.-->

1. Disable the ability to grant tokens
2. Relinquish Vesting Whitelister keys
3. Relinquish Token Controller keys

First, the code at the `Token Controller` address needs to be analyzed. The controller may have been implemented a number of ways as long as it implements the interfaces below. Some of the administrative features may be disabled by default.

```
contract TokenController {
    /// @notice Called when `_owner` sends ether to the MiniMe Token contract
    /// @param _owner The address that sent the ether to create tokens
    /// @return True if the ether is accepted, false if it throws
    function proxyPayment(address _owner) payable returns(bool);

    /// @notice Notifies the controller about a token transfer allowing the
    ///  controller to react if desired
    /// @param _from The origin of the transfer
    /// @param _to The destination of the transfer
    /// @param _amount The amount of the transfer
    /// @return False if the controller does not authorize the transfer
    function onTransfer(address _from, address _to, uint _amount) returns(bool);

    /// @notice Notifies the controller about an approval allowing the
    ///  controller to react if desired
    /// @param _owner The address that calls `approve()`
    /// @param _spender The spender in the `approve()` call
    /// @param _amount The amount in the `approve()` call
    /// @return False if the controller does not authorize the approval
    function onApprove(address _owner, address _spender, uint _amount)
        returns(bool);
}
```


### Relevant Addresses

**BLT**

`MiniMe Vested ERC20 Token: 0x107c4504cd79C5d2696Ea0030a8dD4e92601B82e`

**Vesting Whitelister**

`Bloom Token Sale Contract: 0xB547CC51CE58293E6945bA08d664ce051563d9Ac`

**Token Sale Owner**

`Bloom Multisig: 0x9d217bcBD0Bfae4D7f8f12c7702108D162e3Ab79`

**Controller**

`Unknown Token Controller Contract: 0xfCc9774d0498b2Ab2e53988bCb7c5860DCD3CEb4`

**Token Factory**

`Contract Address: 0xF418Eed07958f018d484B8D43e1af1249CCe0B6E`


### Configuring

Only the `Controller` has the ability to change the `Controller` address. Since the controller is a contract this will only be possible if `changeController` is implemented by the controller implementation.


### Minting

The `Controller` has the ability to mint tokens via the `generateTokens` method. This privilege can be removed by disabling this functionality in the controller contract. This, along with removing the burn method will keep the BLT total supply constant at this implementation address (See Cloning).

```
  /// @notice Generates `_amount` tokens that are assigned to `_owner`
  /// @param _owner The address that will be assigned the new tokens
  /// @param _amount The quantity of tokens generated
  /// @return True if the tokens are generated correctly
  function generateTokens(address _owner, uint _amount
  ) onlyController returns (bool) {
      uint curTotalSupply = totalSupply();
      require(curTotalSupply + _amount >= curTotalSupply); // Check for overflow
      uint previousBalanceTo = balanceOf(_owner);
      require(previousBalanceTo + _amount >= previousBalanceTo); // Check for overflow
      updateValueAtNow(totalSupplyHistory, curTotalSupply + _amount);
      updateValueAtNow(balances[_owner], previousBalanceTo + _amount);
      Transfer(0, _owner, _amount);
      return true;
  }
```

### Burning

The `Controller` has the ability to burn tokens via the `destroyTokens` method. This privilege can be removed by disabling this functionality in the controller contract.

```
  /// @notice Burns `_amount` tokens from `_owner`
  /// @param _owner The address that will lose the tokens
  /// @param _amount The quantity of tokens to burn
  /// @return True if the tokens are burned correctly
  function destroyTokens(address _owner, uint _amount
  ) onlyController returns (bool) {
      uint curTotalSupply = totalSupply();
      require(curTotalSupply >= _amount);
      uint previousBalanceFrom = balanceOf(_owner);
      require(previousBalanceFrom >= _amount);
      updateValueAtNow(totalSupplyHistory, curTotalSupply - _amount);
      updateValueAtNow(balances[_owner], previousBalanceFrom - _amount);
      Transfer(_owner, 0, _amount);
      return true;
  }
```

### Freezing

The `Controller` has the ability to freeze token transfers via the `enableTransfers` method. This privilege can be removed by disabling this functionality in the controller contract.

```
  /// @notice Enables token holders to transfer their tokens freely if true
  /// @param _transfersEnabled True if transfers are allowed in the clone
  function enableTransfers(bool _transfersEnabled) onlyController {
      transfersEnabled = _transfersEnabled;
  }
```

### Cloning

Any user can call `createCloneToken` to deploy a new token based on the token balances of the `BLT` contract at the specified block. This process requires no special privileges so it is unaffected by this proposal.

```
  /// @notice Burns `_amount` tokens from `_owner`
  /// @param _owner The address that will lose the tokens
  /// @param _amount The quantity of tokens to burn
  /// @return True if the tokens are burned correctly
  function destroyTokens(address _owner, uint _amount
  ) onlyController returns (bool) {
      uint curTotalSupply = totalSupply();
      require(curTotalSupply >= _amount);
      uint previousBalanceFrom = balanceOf(_owner);
      require(previousBalanceFrom >= _amount);
      updateValueAtNow(totalSupplyHistory, curTotalSupply - _amount);
      updateValueAtNow(balances[_owner], previousBalanceFrom - _amount);
      Transfer(_owner, 0, _amount);
      return true;
  }
```

### Granting vested tokens

There may have been addresses that were given the ability to grant vested tokens. Bloom does not plan to use the integrated vesting methods of the BLT smart contract in the future so these privileges should be revoked before burning the `Vesting Whitelister` privilege. The grant whitelisting function does not emit an event so the only way to see if an address has token granting privileges is to scan through all BLT transactions. 

```
  function setCanCreateGrants(address _addr, bool _allowed)
           public onlyVestingWhitelister {
    doSetCanCreateGrants(_addr, _allowed);
  }
```

The `Vesting Whitelister` is still set to the `Bloom Token Sale` contract, which is owned by the `Bloom Multisig`. The token sale contract only appears to have used the `grantVestedToken` function in the `allocatePresaleTokens` function.

```
  // @dev Allocate a normal presale grant. Does not necessarily come from a limited pool like the advisor tokens.
  //
  // @param _receiver Recipient of grant
  // @param _amount Total BLT units allocated
  // @param _cliffDate Vesting cliff
  // @param _vestingDate Date that the vesting finishes
  function allocatePresaleTokens(address _receiver, uint256 _amount, uint64 cliffDate, uint64 vestingDate)
           public
           presaleOnly {

    require(_amount <= 10 ** 25); // 10 million BLT. No presale partner will have more than this allocated. Prevent overflows.

    // solhint-disable-next-line not-rely-on-time
    token.grantVestedTokens(_receiver, _amount, uint64(now), cliffDate, vestingDate, true, false);

    NewPresaleAllocation(_receiver, _amount);
  }
```

### Revoking granted tokens

The `Vesting Whitelister` has the ability to revoke a token grant. This is only enabled if the grant was specified as `revokable` on creation and if the `Vesting Whitelister` is also the listed granter. The tokens can either be burned or sent to the receiver specified by the revoker. This functionality will be unavailable once the `Vesting Whitelister` key is burned.

```
  function revokeTokenGrant(address _holder, address _receiver, uint256 _grantId) public onlyVestingWhitelister {
    TokenGrant storage grant = grants[_holder][_grantId];

    require(grant.revokable);
    require(grant.granter == msg.sender); // Only granter can revoke it

    address receiver = grant.burnsOnRevoke ? 0xdead : _receiver;

    uint256 nonVested = nonVestedTokens(grant, uint64(now));

    // remove grant from array
    delete grants[_holder][_grantId];
    grants[_holder][_grantId] = grants[_holder][grants[_holder].length.sub(1)];
    grants[_holder].length -= 1;

    doTransfer(_holder, receiver, nonVested);
  }
```

### Managing active grants

If there are any active token grants, they should not be affected by this change. When tokens are granted they are immediately transferred from the granter to the recipient. Then each time the user tries to transfer some of their token balance, a calculation is performed to determine if the granted tokens have vested and may be released. This calculation does not require the `Vesting Whitelister` to exist or for the granter to exist. Therefore any active grants will not be interrupted by burning all privileged keys.

```
  function transferableTokens(address holder, uint64 time) public constant returns (uint256) {
    uint256 grantIndex = tokenGrantsCount(holder);

    if (grantIndex == 0) return balanceOf(holder); // shortcut for holder without grants

    // Iterate through all the grants the holder has, and add all non-vested tokens
    uint256 nonVested = 0;
    for (uint256 i = 0; i < grantIndex; i++) {
      nonVested = nonVested.add(nonVestedTokens(grants[holder][i], time));
    }

    // Balance - totalNonVested is the amount of tokens a holder can transfer at any given time
    return balanceOf(holder).sub(nonVested);
  }
```


### Claiming Tokens

If tokens or ether are sent directly to this contract address the `Controller` has the ability to recover the funds. If the controller key is burned there will be no way to recover tokens or ether mistakenly sent to this contract.

```
  /// @notice This method can be used by the controller to extract mistakenly
  ///  sent tokens to this contract.
  /// @param _token The address of the token contract that you want to recover
  ///  set to 0 in case you want to extract ether.
  function claimTokens(address _token) onlyController {
      if (_token == 0x0) {
          controller.transfer(this.balance);
          return;
      }

      MiniMeToken token = MiniMeToken(_token);
      uint balance = token.balanceOf(this);
      token.transfer(controller, balance);
      ClaimedTokens(_token, controller, balance);
  }
```

## Rationale
<!--The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->
All BLT contract code was reviewed and the relevant state variables were read from [etherscan](https://etherscan.io/address/0x107c4504cd79c5d2696ea0030a8dd4e92601b82e#readContract).

The actions outlined in the specification section are motivated by the desire to make BLT entirely trustless while being cautious to ensure we do not accidentally disable the token functionality.

## Backwards Compatibility
<!--All BLIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The BLIP must explain how the author proposes to deal with these incompatibilities. BLIP submissions without a sufficient backwards compatibility treatise may be rejected outright.-->
This change is fully backward compatible with all third parties that interface with the BLT token. The contract address and ERC20 function interfaces are not changing.

## Test Cases
<!--Test cases for an implementation are mandatory for BLIPs that are affecting governance changes. Other BLIPs can choose to include links to test cases if applicable.-->
No core ERC20 features are affected. Modifying the `Token Controller` contract must be done carefully. If a new `Token Controller` contract is implemented that fails to implement the required methods all BLT functionality will be frozen. We will do a test run of the intended action both on a local testRPC and on a public testnet such as Rinkeby.

## Implementation
<!--The implementations must be completed before any BLIP is given status "Final", but it need not be completed before the BLIP is accepted. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of "rough consensus and running code" is still useful when it comes to resolving many discussions of API details.-->
Implemenation will be outlined upon review of this draft. This proposal will be updated with implementation plans prior to execution.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/). Based on the Ethereum Improvement Proposal template.
