Disclosure

Purpose:
This report is submitted for educational and defensive security purposes. The goal is to demonstrate a critical vulnerability in ERC20 token implementations where permit() can bypass emergency pause mechanisms. No live exploitation or malicious activity was conducted on mainnet or any user funds.

Responsible Disclosure:
All findings are responsibly disclosed. The vulnerability was identified in a test or local environment, and all proof-of-concept code is sanitized to avoid risk.

Vendor/Project Notification:
The affected project/vendor has been informed of the issue through the appropriate channels prior to publication. This write-up does not contain private keys, sensitive user data, or any actionable exploit instructions that could be misused.

Risk Mitigation:
Sanitized code examples and recommendations are provided to help developers understand the vulnerability and apply proper mitigations, including:

Applying whenNotPaused to the permit() function.

Overriding _approve() to enforce pause checks.

Intended Audience:
This repository and report are intended for:

Security researchers

Developers of ERC20 and upgradeable contracts

Educational purposes in security audits

Disclaimer:
This write-up does not constitute financial advice or encouragement to exploit any live contracts. Any reproduction of these findings should be done in isolated test environments only.
