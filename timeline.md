# Disclosure Timeline

This document outlines the responsible discovery and disclosure timeline for the
"Pause Mechanism Bypass via ERC20Permit Allowance Mutation" vulnerability.

All activities were performed in local or test environments only.
No mainnet exploitation or interaction with user funds occurred.

---

## Timeline

### Day 0 — Initial Research
- Reviewed ERC20Permit + PausableUpgradeable interaction patterns
- Identified potential invariant violation when `permit()` is callable during pause
- Hypothesized that allowance mutation during pause may enable post-pause attacks

### Day 1 — Local Reproduction
- Built a local Hardhat test environment
- Deployed an upgradeable ERC20 contract with:
  - ERC20Permit
  - PausableUpgradeable
  - UUPS / ERC1967 proxy
- Confirmed that `permit()` succeeds while the contract is paused

### Day 2 — Proof of Concept Development
- Implemented a sanitized proof-of-concept test:
  - Pause contract
  - Execute `permit()`
  - Observe allowance mutation
- Verified invariant violation without transferring funds
- Gas reports and console output collected

### Day 3 — Impact Assessment
- Analyzed real-world implications:
  - Emergency pause invariant violation
  - Post-pause attack preparation
- Classified severity as **High** due to:
  - Security boundary bypass
  - Attack chaining potential
  - No user interaction required after pause lift

### Day 4 — Responsible Disclosure Attempt
- Attempted disclosure via recognized bug bounty platforms
- Encountered identity verification (KYC) restrictions
- Reached out to the project/vendor directly for notification

### Day 5 — Public Sanitized Disclosure
- Prepared a fully sanitized public write-up
- Removed:
  - Exploit automation
  - Mainnet addresses
  - Sensitive deployment parameters
- Published this repository for:
  - Defensive security education
  - Developer awareness
  - Professional portfolio demonstration

---

## Disclosure Status

- [x] Vulnerability reproduced locally
- [x] Proof-of-concept validated
- [x] Vendor notified via appropriate channels
- [x] Public disclosure sanitized
- [x] No exploitation performed

---

## Ethical Statement

This research follows responsible disclosure principles.
The author did not exploit this issue in production systems
and actively avoided any action that could cause harm to users or protocols.

