# Pause Mechanism Bypass via ERC20Permit Allowance Mutation

## Summary

A security design flaw was identified in an ERC20 token implementation where the `permit()` function remains callable while the contract is in a paused state.

Although no direct token transfer is possible during pause, this behavior allows mutation of allowances during an emergency shutdown, violating the core invariant of pause mechanisms and enabling post-pause attack preparation.

This issue is related to the interaction between `ERC20Permit` and `PausableUpgradeable` when pause checks are not consistently enforced.

---

## Affected Design Pattern

- ERC20Upgradeable
- ERC20Permit
- PausableUpgradeable
- UUPS Proxy (ERC1967)

---

## Vulnerability Class

- Emergency Mechanism Invariant Violation
- Authorization State Manipulation
- ERC20Permit Misconfiguration

---

## Severity

**High**

---

## Impact

While the contract is paused:

- Allowances can still be created or modified via `permit()`
- Attackers may pre-authorize spending without the token holder’s immediate awareness
- Once the protocol is unpaused, approved tokens can be transferred without further user interaction

This behavior undermines the intent of emergency pause mechanisms, which are expected to freeze *all* sensitive state mutations during incident response.

> ⚠️ Note: This issue does **not** allow token transfer during the paused state.

---

## Root Cause

The `permit()` function is callable during pause and internally updates allowances without enforcing `whenNotPaused`.

In implementations where `transferFrom()` is correctly blocked during pause, the allowance mutation still persists and becomes effective immediately after unpause.

---

## Proof of Concept (Sanitized)

A local Hardhat-based test demonstrates that:

1. The contract is paused
2. `permit()` executes successfully
3. The allowance is updated despite the paused state

No mainnet interaction or real-user exploitation was performed.

