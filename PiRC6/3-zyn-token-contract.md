# 3. ZYN Token Contract

Implements PiRC1 token design with burn, staking, and governance.

## 3.1 Core Functions

```rust
// === TOKEN STANDARD (SPL-compatible) ===
fn initialize(env: Env, admin: Address, decimals: u32, name: String, symbol: String);
fn balance_of(env: Env, account: Address) -> i128;
fn transfer(env: Env, from: Address, to: Address, amount: i128);
fn transfer_from(env: Env, spender: Address, from: Address, to: Address, amount: i128);
fn approve(env: Env, owner: Address, spender: Address, amount: i128, expiration_ledger: u32);
fn allowance(env: Env, owner: Address, spender: Address) -> i128;

// === BURN MECHANISM (PiRC1) ===
fn burn(env: Env, from: Address, amount: i128);
fn burn_from(env: Env, spender: Address, from: Address, amount: i128);

// === MERCHANT STAKING (PiRC1) ===
fn stake(env: Env, merchant: Address, amount: i128);
fn unstake(env: Env, merchant: Address, amount: i128);
fn get_stake(env: Env, merchant: Address) -> i128;
fn slash_stake(env: Env, merchant: Address, amount: i128); // admin/governance only

// === GOVERNANCE (PiRC1) ===
fn propose(env: Env, proposer: Address, title: String, action: ProposalAction);
fn vote(env: Env, voter: Address, proposal_id: u64, support: bool);
fn execute_proposal(env: Env, proposal_id: u64);
fn get_proposal(env: Env, proposal_id: u64) -> Proposal;
```

## 3.2 Burn on Transfer

Every ZYN transfer automatically burns 0.5%:

```rust
fn transfer(env: Env, from: Address, to: Address, amount: i128) {
    let burn_amount = amount / 200; // 0.5%
    let transfer_amount = amount - burn_amount;

    // Transfer to recipient
    internal_transfer(&env, &from, &to, transfer_amount);

    // Burn
    internal_burn(&env, &from, burn_amount);

    // Events
    env.events().publish(
        (Symbol::new("zyn"), Symbol::new("transfer")),
        (from.to_val(), to.to_val(), transfer_amount.to_val())
    );
    env.events().publish(
        (Symbol::new("zyn"), Symbol::new("burn")),
        (from.to_val(), burn_amount.to_val())
    );
}
```

## 3.3 Staking Mechanics

```rust
fn stake(env: Env, merchant: Address, amount: i128) {
    merchant.require_auth();

    // Transfer ZYN from merchant to contract
    internal_transfer(&env, &merchant, &env.current_contract_address(), amount);

    // Update stake
    let current = get_stake(&env, &merchant);
    set_stake(&env, &merchant, current + amount);

    env.events().publish(
        (Symbol::new("zyn"), Symbol::new("stake")),
        (merchant.to_val(), amount.to_val())
    );
}
```

## 3.4 Governance

Proposals require 1000 ZYN stake to submit. Voting power = staked ZYN. Quorum = 10% of total stake. Execution requires >50% yes votes.

| Phase | Duration | Action |
|---|---|---|
| **Proposal** | 1 day | Proposer submits with 1000 ZYN deposit |
| **Voting** | 3 days | ZYN stakers vote yes/no |
| **Execution** | 2 days | If passed, anyone can execute |

Next: [`4-Subscription-Contract`](4-subscription-contract.md)
