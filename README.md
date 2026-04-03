# daml-learning

A structured DAML learning journey building toward smart contract development on the Canton Network. Each module covers a specific concept, progressing from basic templates to full multi-party applications.

---

## What is DAML?

DAML (Digital Asset Modeling Language) is a smart contract language designed for building multi-party workflows on distributed ledgers. It powers the Canton Network — an enterprise-grade blockchain used by major financial institutions. Every contract in DAML enforces who can see it, who can act on it, and what those actions produce.

---

## Modules

### 1. `UserProfile.daml`
**Concept: Templates, fields, signatories, and choices**

The starting point. Models a simple user profile with a single party as the owner. Includes an `UpdateName` choice that creates a new version of the contract with an updated name. Introduces the create/archive lifecycle.

---

### 2. `Connection.daml`
**Concept: Multiple parties, observers, propose/accept pattern**

Models a connection request between two users — a sender and a receiver. The sender creates the request, the receiver can accept it. Introduces the `observer` keyword, which lets a party see a contract without being able to act on it.

---

### 3. `TokenOffer.daml`
**Concept: Cross-template creation, propose/accept at scale**

An issuer offers tokens to a recipient. The recipient can accept (creating a `Token` contract) or reject (archiving the offer). Demonstrates how one template can create another and how choices return different contract types.

---

### 4. `Guardsandcondition.daml`
**Concept: Assertions and conditional logic**

A vault deposit contract where an owner can deposit or withdraw funds. The `Withdraw` choice uses `assertMsg` to fail the transaction if the withdrawal amount exceeds the balance. Introduces guard conditions that enforce business rules at the contract level.

---

### 5. `FetchContract.daml`
**Concept: Fetching contracts and composing logic across templates**

An `Account` template that can return its own balance, and a `Payment` template that fetches an account, checks the balance, and deducts the payment amount. Introduces the `fetch` keyword and the `<-` binding pattern for working with ledger state inside choices.

---

### 6. `CantonOnboarding.daml` + `TestCantonOnboarding.daml`
**Concept: Full multi-party application with scripts**

The first complete app. Models a Canton platform onboarding flow:
- A user submits a `UserApplication`
- An admin approves it, creating an `ApprovedUser`
- The admin allocates tokens, creating a `UserWallet`
- The admin can top up the wallet via `Deposit`

The test script runs the full flow end-to-end, allocating parties, exercising choices, and verifying the final wallet state using `queryContractId`.

**Why it matters:** Directly mirrors real onboarding flows used in Canton ecosystem apps.

---

### 7. `LoanRequest.daml` + `TestLoanRequest.daml`
**Concept: Date types, dual signatories, repayment lifecycle**

A borrower requests a loan from a lender with a specified amount and reason. The lender can approve (creating a `LoanAgreement` with a due date) or reject. The borrower can repay the loan, archiving the agreement.

Introduces the `Date` type and contracts that require two parties as signatories — meaning both must authorize the contract's existence.

---

### 8. `MarketPlace.daml` + `TestMarketPlace.daml`
**Concept: Three-template chain, buyer/seller workflow**

A full marketplace flow:
- A seller creates a `MarketPlaceListing`
- A buyer exercises `MakeOffer`, creating a `PendingOffer`
- The seller accepts, creating a `SaleReceipt` visible to both parties
- The seller can also cancel the listing or decline any offer

Demonstrates chaining three templates together and how contract visibility is controlled at each step.

---

### 9. `MultiSigTreasury.daml` + `TestMultiSigTreasury.daml`
**Concept: Multi-signature authorization pattern**

A treasury that requires two admins to authorize before funds are released:
- `admin1` proposes a release
- `admin2` must countersign it, creating a `TreasuryRelease`
- Only then can the `recipient` execute the release

Demonstrates the multi-sig pattern used in institutional finance on Canton. Also illustrates a key DAML privacy concept: a party must be an observer or signatory on a contract before they can exercise choices on it.

---

## Key DAML Concepts Covered

| Concept | Where it appears |
|---|---|
| Templates & fields | UserProfile |
| Signatories & observers | Connection, TokenOffer |
| Choices & return types | All modules |
| Propose & accept pattern | TokenOffer, MarketPlace |
| Guard conditions (`assertMsg`) | Guardsandcondition |
| Fetching contracts | FetchContract |
| Date types | LoanRequest |
| Dual signatories | LoanRequest, MultiSigTreasury |
| Cross-template creation | CantonOnboarding, MarketPlace |
| DAML Scripts & testing | All test modules |
| Multi-sig authorization | MultiSigTreasury |
| Contract visibility & privacy | MultiSigTreasury |

---

## Setup

**Prerequisites:**
- DAML SDK installed (`daml --version`)
- VS Code with the DAML extension

**Run the project:**
```bash
daml studio
```

**Run a specific script:**
Open any `Test*.daml` file in VS Code, place your cursor inside the script function, and the Script Results panel will show the full transaction trace.

---

## About

Built as part of a structured DAML learning path toward smart contract development on the Canton Network. Focused on real-world financial use cases: onboarding, lending, marketplace, and treasury management — all patterns used in production Canton applications.
