# Pause Mechanism Bypass via ERC20Permit Allowance Mutation

## Summary
A critical design flaw was identified in an ERC20 token implementation where the `permit()` function remains callable while the contract is paused.  
This allows mutation of allowances during an emergency pause, violating the core invariant of pause mechanisms and enabling post-pause attack preparation.

## Affected Pattern
- ERC20Upgradeable
- ERC20Permit
- PausableUpgradeable
- UUPS Proxy (ERC1967)

## Vulnerability Class
- Access Control Bypass
- Emergency Mechanism Invariant Violation
- ERC20Permit Misuse

## Severity
**High**

## Impact
Even when the protocol is paused:
- Allowances can still be created or modified via `permit`
- Attackers can pre-authorize spending
- Once unpaused, funds can be drained without further user interaction

This defeats the purpose of emergency pause mechanisms intended to freeze *all* state-mutating actions.

## Root Cause
The `permit()` function is not protected by `whenNotPaused`, allowing allowance mutation during paused state.

## Proof of Concept
A Hardhat test demonstrates that:
1. Contract is paused
2. `permit()` succeeds
3. Allowance is updated despite pause

See: `/poc/pause-permit.test.js`

## Recommendation
Apply `whenNotPaused` modifier to `permit()` or override `_approve()` with pause checks.

## Disclosure
This report is published for educational and defensive security purposes.
No exploitation was performed on mainnet.

