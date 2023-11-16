
# Notional #2 contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
Arbitrum, Mainnet, Optimism
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
There are three admin roles within the vault system:
1. Owner: is the same as owner of the overall Notional system, capabale of upgrades and changing governance settings. Is able to restore the vault after an emergency exit. This account is TRUSTED.
2. Emergency Exit: this account can execute an emergency exit on the SingleSidedLP vault. An emergency exit is RESTRICTED to withdrawing funds proportionally from the LP pool and locking the vault. This account should not be able to do anything other than call this function. This account may potentially cause a denial of service to account holders but should not be able to move funds off the contract outright.
3. Reward Reinvestment: this account can call claimRewardTokens and reinvestReward on the SingleSidedLP vault. This account is RESTRICTED to those functions only. This account should only be able to sell reward tokens in exchange for pool tokens and increase the overall number of LP pool tokens on the contract. This account will be trusted to set appropriate slippage limits when executing trades.
___

### Q: Is the admin/owner of the protocol/contracts TRUSTED or RESTRICTED?
Restricted, can withdraw funds from LP pools in case of emergency but cannot transfer them from the contracts arbitrarily.
___

### Q: Are there any additional protocol roles? If yes, please explain in detail:
Treasury Manager is allowed to claim incentives and reinvest tokens back into the vault. This will be listed as the reward reinvestment contract.
___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
No special EIP considerations.
___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
1. Chainlink oracles heartbeats are not explicity validated within the TradingModule, we have a global restriction on chainlink heartbeats set within TradingModule.setMaxOracleFreshness. This has been reported an issue in prior audits and we have not considered it valid in the past.

2. Use of transfer() for ETH is well understood and used to mitigate re-entrancy.

3. The "Mixin" contracts are inherited and use only immutable parameters. Storage slots only reside on the BaseStrategyVault and VaultStorage contracts, so there is limited potential for storage gap issues for future upgrades. This has been reported in the past and we would consider this a low or informational finding.


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
External protocol integrations include Balancer and Curve. It is understood that Curve does not hold any ability to pause or emergency withdraw after a pool has been live for 30 days. We would not list vaults on pools that have been live less than this period of time. https://dao.curve.fi/emergencymembers

Balancer can call enableRecoveryMode which will essentially lock a SingleSidedLP vault. If this is the case, admins will likely need to respond with an emergencyExit or whitelisting a approporiate TradingModule functionality to allow for trading out of a prortional exit. https://docs.balancer.fi/concepts/governance/emergency.html#multisigs

For the cross currency vault, the counterparty is Notional itself so we would not consider those external contracts.
___

### Q: Do you expect to use any of the following tokens with non-standard behaviour with the smart contracts?
No
___

### Q: Add links to relevant protocol resources
https://docs.notional.finance/notional-v3/leveraged-vaults/what-are-leveraged-vaults

https://github.com/notional-finance/leveraged-vaults/blob/tests/composable-pool/README.md


These vault contracts within the Notional V3 Leveraged Vault framework. Some familiarity with that vault framework is required when auditing this code.

Latest Code:
https://github.com/notional-finance/contracts-v2/tree/audit/sherlock-audit-fixes
___






# Audit scope


[leveraged-vaults @ cf17af56ca489bc2729e8a09567af36a0c723192](https://github.com/notional-finance/leveraged-vaults/tree/cf17af56ca489bc2729e8a09567af36a0c723192)
- [leveraged-vaults/contracts/vaults/BalancerComposableAuraVault.sol](leveraged-vaults/contracts/vaults/BalancerComposableAuraVault.sol)
- [leveraged-vaults/contracts/vaults/BalancerWeightedAuraVault.sol](leveraged-vaults/contracts/vaults/BalancerWeightedAuraVault.sol)
- [leveraged-vaults/contracts/vaults/CrossCurrencyVault.sol](leveraged-vaults/contracts/vaults/CrossCurrencyVault.sol)
- [leveraged-vaults/contracts/vaults/Curve2TokenConvexVault.sol](leveraged-vaults/contracts/vaults/Curve2TokenConvexVault.sol)
- [leveraged-vaults/contracts/vaults/balancer/BalancerSpotPrice.sol](leveraged-vaults/contracts/vaults/balancer/BalancerSpotPrice.sol)
- [leveraged-vaults/contracts/vaults/balancer/mixins/AuraStakingMixin.sol](leveraged-vaults/contracts/vaults/balancer/mixins/AuraStakingMixin.sol)
- [leveraged-vaults/contracts/vaults/balancer/mixins/BalancerPoolMixin.sol](leveraged-vaults/contracts/vaults/balancer/mixins/BalancerPoolMixin.sol)
- [leveraged-vaults/contracts/vaults/common/BaseStrategyVault.sol](leveraged-vaults/contracts/vaults/common/BaseStrategyVault.sol)
- [leveraged-vaults/contracts/vaults/common/SingleSidedLPVaultBase.sol](leveraged-vaults/contracts/vaults/common/SingleSidedLPVaultBase.sol)
- [leveraged-vaults/contracts/vaults/common/StrategyUtils.sol](leveraged-vaults/contracts/vaults/common/StrategyUtils.sol)
- [leveraged-vaults/contracts/vaults/common/VaultStorage.sol](leveraged-vaults/contracts/vaults/common/VaultStorage.sol)
- [leveraged-vaults/contracts/vaults/curve/ConvexStakingMixin.sol](leveraged-vaults/contracts/vaults/curve/ConvexStakingMixin.sol)
- [leveraged-vaults/contracts/vaults/curve/Curve2TokenPoolMixin.sol](leveraged-vaults/contracts/vaults/curve/Curve2TokenPoolMixin.sol)


