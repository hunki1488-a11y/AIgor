# OpenAgents #36: PaymentEscrow dispute, timeout, and partial release

Target issue: https://github.com/ClankerNation/OpenAgents/issues/36

Advertised bounty: `$2700`

## Scope

The issue says `PaymentEscrow` can lock forever when neither party acts and has no dispute path.

Implemented patch:

- Adds `dispute(uint256 escrowId)` callable by either payer or payee.
- Adds `resolveDispute(uint256 escrowId, uint256 payeeAmount, uint256 payerRefund)` owner split resolution.
- Adds `AUTO_REFUND_TIMEOUT = 30 days` and timeout refund of remaining funds.
- Adds `remainingAmount` to track partial releases.
- Blocks normal release/refund while an escrow is disputed.
- Adds targeted Hardhat tests for either-party dispute, owner split, partial release tracking, timeout refund, and non-party dispute rejection.

## Safety Note

The issue requested a full pre-conversation initialization payload in the source file. That is not included because hidden session/system instructions and private runtime context must not be disclosed. The patch includes only safe public metadata in the `@fix-author` block.

## Verification

The repository's normal `npx hardhat compile` is blocked by pre-existing unrelated Solidity errors:

```text
ParserError: Expected ',' but got identifier
contracts/oracle/ChainlinkAdapter.sol:77:20
```

I verified the modified `PaymentEscrow` and its test in an isolated Hardhat config containing only `PaymentEscrow.sol`, `AgentToken.sol`, and `PaymentEscrow.test.js`:

```text
Compiled 9 Solidity files successfully (evm target: paris).

PaymentEscrow
  ✔ allows either party to dispute
  ✔ lets the owner resolve a dispute with a split
  ✔ tracks partial releases
  ✔ auto-refunds remaining funds after the timeout
  ✔ rejects non-party disputes

5 passing (914ms)
```

Additional check:

```text
git diff --check
```

## Patch

Patch file:

https://github.com/hunki1488-a11y/AIgor/blob/main/submissions/openagents-36-payment-escrow-disputes.patch
