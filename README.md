# Madara Escrow

A modified [Madara](https://github.com/keep-starknet-strange/madara) app chain, for escrows (locked payments)

This Madara App Chain contains a pallet which allows users to create [secure escrows](./crates/pallets/escrow/) that keep funds locked in a merchant's account until the off-chain goods/services are confirmed to be received. Each payment/escrow gets assigned its own *judge* that can help resolve any disputes between the two parties.

## Use case

This Madara App Chain focuses on use cases where an intermediate step is required before the recipient of a payment has access to money, such as payment for off-chain products and services, for supply chain management, for industry banking, for regulated markets, cross-border payments, among others.

## Getting Started

To build and user this app chain, follow the instructions on [Madara docs](https://github.com/keep-starknet-strange/madara-app-chain-template).

_This escrow pallet needs other helper pallets (adapted from other projects), available in the same [pallets folder](./crates/pallets/)_

This Escrow Chain use the following:

### Terminology
- **Created**: A payment has been created and the amount arrived to its destination but it's locked.
- **NeedsReview**: The payment has bee disputed and is awaiting settlement by a judge.
- **IncentivePercentage**: A small share of the payment amount is held in escrow until a payment is completed/cancelled. The Incentive Percentage represents this value.
- **Resolver Account**: A resolver account is assigned to every payment created, this account has the privilege to cancel/release a payment that has been disputed.
- **Remark**: The pallet allows to create payments by optionally providing some extra(limited) amount of bytes, this is referred to as Remark. This can be used by a marketplace to separate/tag payments.
- **CancelBufferBlockLength**: This is the time window where the recipient can dispute a cancellation request from the payment creator.

### Actions
- **pay**: Create an payment for the given currencyid/amount
- **pay_with_remark**: Create a payment with a remark, can be used to tag payments
- **release**: Release the payment amount to recipent
- **cancel**: Allows the recipient to cancel the payment and release the payment amount to creator
- **resolve_release_payment**: Allows assigned judge to release a payment
- **resolve_cancel_payment**:  Allows assigned judge to cancel a payment
- **request_refund**: Allows the creator of the payment to trigger cancel with a buffer time.
- **claim_refund**: Allows the creator to claim payment refund after buffer time
- **dispute_refund**: Allows the recipient to dispute the payment request of sender
- **request_payment**: Create a payment that can be completed by the sender using the `accept_and_pay` extrinsic.
- **accept_and_pay**: Allows the sender to fulfill a payment request created by a recipient

### Types
- **PaymentDetail** struct: Stores information about the payment/escrow. A "payment" in virto network is similar to an escrow, it is used to guarantee proof of funds and can be released once an agreed upon condition has reached between the payment creator and recipient. The payment lifecycle is tracked using the state field.

- **PaymentState** enum: Tracks the possible states that a payment can be in. When a payment is 'completed' or 'cancelled' it is removed from storage and hence not tracked by a state.