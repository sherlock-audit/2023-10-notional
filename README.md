
# Notional #2 contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
Arbitrum, Mainnet, Optimism, potentially any chain where Balancer V2 has deployed
___

### Q: Which ERC20 tokens do you expect will interact with the smart contracts? 
any
___

### Q: Which ERC721 tokens do you expect will interact with the smart contracts? 
none
___

### Q: Which ERC777 tokens do you expect will interact with the smart contracts? 
none
___

### Q: Are there any FEE-ON-TRANSFER tokens interacting with the smart contracts?

No, fee on transfer is prohibited in the leveraged vault framework
___

### Q: Are there any REBASING tokens interacting with the smart contracts?

No
___

### Q: Are the admins of the protocols your contracts integrate with (if any) TRUSTED or RESTRICTED?
Restricted
___

### Q: Is the admin/owner of the protocol/contracts TRUSTED or RESTRICTED?
Restricted, can withdraw funds from Balancer pools in case of emergency but cannot transfer them from the contracts arbitrarily.
___

### Q: Are there any additional protocol roles? If yes, please explain in detail:
Treasury Manager is allowed to claim incentives and reinvest tokens back into the vault.
___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
No special EIP considerations.
___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
This Balancer Vault exists within the Notional V3 Leveraged Vault framework. Some familiarity with that vault framework is required when auditing this code.

Latest Code:
https://github.com/notional-finance/contracts-v2/tree/audit/sherlock-audit-fixes
___

### Q: Please provide links to previous audits (if any).
https://audits.sherlock.xyz/contests/2
https://audits.sherlock.xyz/contests/31

Full Audit List:
https://github.com/notional-finance/contracts-v2/tree/master/audits
___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)?
There is a reward reinvestment bot that calls `reinvestRewards`
___

### Q: In case of external protocol integrations, are the risks of external contracts pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality.
No, Balancer V2 is assumed to be a trusted integration.
___

### Q: Do you expect to use any of the following tokens with non-standard behaviour with the smart contracts?
No
___

### Q: Add links to relevant protocol resources
https://docs.notional.finance/notional-v3/leveraged-vaults/what-are-leveraged-vaults
___



# Audit scope


[leveraged-vaults @ ea26c0602f6b049618e668f8f0b5fda579f0312d](https://github.com/notional-finance/leveraged-vaults/tree/ea26c0602f6b049618e668f8f0b5fda579f0312d)
- [leveraged-vaults/contracts/vaults/BalancerComposableAuraVault.sol](leveraged-vaults/contracts/vaults/BalancerComposableAuraVault.sol)
- [leveraged-vaults/contracts/vaults/balancer/BalancerVaultTypes.sol](leveraged-vaults/contracts/vaults/balancer/BalancerVaultTypes.sol)
- [leveraged-vaults/contracts/vaults/balancer/external/ComposableAuraHelper.sol](leveraged-vaults/contracts/vaults/balancer/external/ComposableAuraHelper.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/BalancerConstants.sol](leveraged-vaults/contracts/vaults/balancer/internal/BalancerConstants.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/math/ComposableOracleMath.sol](leveraged-vaults/contracts/vaults/balancer/internal/math/ComposableOracleMath.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/math/FixedPoint.sol](leveraged-vaults/contracts/vaults/balancer/internal/math/FixedPoint.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/math/Math.sol](leveraged-vaults/contracts/vaults/balancer/internal/math/Math.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/math/StableMath.sol](leveraged-vaults/contracts/vaults/balancer/internal/math/StableMath.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/pool/BalancerComposablePoolUtils.sol](leveraged-vaults/contracts/vaults/balancer/internal/pool/BalancerComposablePoolUtils.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/pool/BalancerUtils.sol](leveraged-vaults/contracts/vaults/balancer/internal/pool/BalancerUtils.sol)
- [leveraged-vaults/contracts/vaults/balancer/internal/reward/ComposableAuraRewardUtils.sol](leveraged-vaults/contracts/vaults/balancer/internal/reward/ComposableAuraRewardUtils.sol)
- [leveraged-vaults/contracts/vaults/balancer/mixins/AuraStakingMixin.sol](leveraged-vaults/contracts/vaults/balancer/mixins/AuraStakingMixin.sol)
- [leveraged-vaults/contracts/vaults/balancer/mixins/BalancerComposablePoolMixin.sol](leveraged-vaults/contracts/vaults/balancer/mixins/BalancerComposablePoolMixin.sol)
- [leveraged-vaults/contracts/vaults/balancer/mixins/BalancerPoolMixin.sol](leveraged-vaults/contracts/vaults/balancer/mixins/BalancerPoolMixin.sol)

