# daml-learning

A structured DAML learning journey building toward smart contract development on the Canton Network. Covers everything from basic functions and type systems to full multi-party smart contract applications with scripts.

---

## What is DAML?

DAML (Digital Asset Modeling Language) is a smart contract language designed for building multi-party workflows on distributed ledgers. It powers the Canton Network — an enterprise-grade blockchain used by major financial institutions. Every contract in DAML enforces who can see it, who can act on it, and what those actions produce.

---

## Part 1 — Functional Programming Foundations

These modules cover DAML as a functional programming language before touching smart contracts. Understanding these concepts is what separates developers who can write DAML from those who understand it.

### `HelloWorld.daml`
**Concept: Functions, text concatenation, basic scripts**

The very first step. A simple function that takes a `Text` argument and returns a greeting. Introduces function signatures, the `<>` string concatenation operator, and running code via DAML scripts.

---

### `Functions.daml`
**Concept: Function signatures, multiple arguments, Decimal arithmetic**

Three simple functions — `increment`, `add`, and `area_form` — demonstrating how DAML handles typed function signatures, multiple arguments via currying, and `Decimal` arithmetic. The foundation for all logic in DAML contracts.

---

### `Lab3.daml`
**Concept: Type classes, data types, deriving, equality and ordering**

Introduces DAML's type system at a deeper level:
- Defines a custom `Person` data type with `name` and `email` fields
- Derives `Eq`, `Ord`, and `Show` automatically — enabling comparison and debugging
- Defines a `Reachable` type class and implements an instance for `Person`
- Demonstrates that type classes in DAML work like interfaces — defining behaviour that different types can implement differently

---

### `Lab4.daml`
**Concept: Guards, case expressions, if/else, Either type, foldl, lambdas**

The most comprehensive functional programming lab:
- Three different ways to write conditional logic: `if/else`, `case`, and guards (`|`)
- The `Either` type for safe error handling — `Left` for errors, `Right` for success
- Heron's formula for triangle area using `DA.Math.sqrt`, `foldl`, and lambda functions
- Validates triangle input with multiple guard conditions before computing

This is production-style functional code — the same patterns appear in real DAML contract logic.

---

### `Lab5.daml`
**Concept: map, foldr, tuples, DA.Text, term frequency**

Advanced list processing and functional composition:
- Works with a list of `(Text, Text)` tuples representing song lyrics
- Uses `map` with field accessor syntax (`._1`, `._2`) to extract tuple elements
- Implements term frequency calculation two ways — once with `map` and once with `foldr`
- Uses `DA.Text.words` to split strings and `List.filter` to count occurrences

Demonstrates the kind of data transformation logic used in analytics and reporting contracts.

---

### `NewLab6.daml`
**Concept: Type classes with multiple instances, Optional, polymorphism**

The most advanced functional programming module:
- Defines a `SafeAccount` type class with a `safeQuery` method that returns `Optional Decimal`
- Returns `Some balance` if the caller's ID matches, `None` if not — safe access without exceptions
- Defines a `Redeemable` type class implemented by both `AirtravelPoint` and `DiningPoint`
- Each point type redeems at a different rate (10x for travel, 5x for dining)
- Combines both to compute a total redeemed value

This pattern — multiple types sharing a common interface — is exactly how Canton ecosystem contracts handle different asset types uniformly.

---

## Part 2 — Smart Contract Development

Independent projects and coursework labs building real DAML smart contracts with multi-party workflows and test scripts.

### Independent Projects

#### 1. `UserProfile.daml`
**Concept: Templates, fields, signatories, choices**

The first smart contract. A user profile with an `UpdateName` choice that creates a new version of the contract. Introduces the create/archive lifecycle fundamental to all DAML contracts.

---

#### 2. `Connection.daml`
**Concept: Multiple parties, observers, propose/accept pattern**

A connection request between two users. The sender creates the request, the receiver accepts it. Introduces `observer` — a party that can see a contract but cannot act on it unless a choice grants them permission.

---

#### 3. `TokenOffer.daml` + `TestTokenOffer.daml`
**Concept: Cross-template creation, propose/accept at scale**

An issuer offers tokens to a recipient who can accept (creating a `Token`) or reject. Demonstrates how one template creates another and how choices return different contract types.

---

#### 4. `Guardsandcondition.daml`
**Concept: Assertions and conditional logic**

A vault deposit contract with `Deposit` and `Withdraw` choices. `Withdraw` uses `assertMsg` to fail the transaction if the amount exceeds the balance — enforcing business rules at the contract level.

---

#### 5. `FetchContract.daml`
**Concept: Fetching contracts, composing logic across templates**

An `Account` that returns its own balance, and a `Payment` that fetches an account, checks the balance, asserts it's sufficient, and deducts the payment. Introduces `fetch` and the `<-` binding pattern for reading ledger state inside choices.

---

#### 6. `CantonOnboarding.daml` + `TestCantonOnboarding.daml`
**Concept: Full multi-party application with scripts**

A complete Canton platform onboarding flow:
- User submits a `UserApplication`
- Admin approves it, creating an `ApprovedUser`
- Admin allocates tokens, creating a `UserWallet`
- Admin tops up the wallet via `Deposit`

**Why it matters:** Directly mirrors real onboarding flows used in Canton ecosystem apps including Cantor8.

---

#### 7. `LoanRequest.daml` + `TestLoanRequest.daml`
**Concept: Date types, dual signatories, repayment lifecycle**

A borrower requests a loan from a lender. The lender approves with a `dueDate` (creating a `LoanAgreement`) or rejects. The borrower repays, archiving the agreement. Introduces `Date` types and contracts requiring two signatories.

---

#### 8. `MarketPlace.daml` + `TestMarketPlace.daml`
**Concept: Three-template chain, buyer/seller workflow**

A full marketplace:
- Seller creates a `MarketPlaceListing`
- Buyer exercises `MakeOffer` → creates `PendingOffer`
- Seller accepts → creates `SaleReceipt` visible to both parties

Demonstrates chaining three templates and controlling visibility at each step.

---

#### 9. `MultiSigTreasury.daml` + `TestMultiSigTreasury.daml`
**Concept: Multi-signature authorization pattern**

A treasury requiring two admins to authorize before funds are released:
- `admin1` proposes a release
- `admin2` countersigns → creates `TreasuryRelease`
- Only then can `recipient` execute

Illustrates a core Canton privacy lesson: a party must be a signatory or observer before they can exercise choices on a contract.

---

### Coursework Labs

#### `Lab1.daml` + `TestLab1.daml`
Introductory smart contract lab covering template structure, party authorization, and basic choice patterns.

---

#### `Lab2.daml` + `TestLab2.daml`
**Customer Loyalty Program (CLP)** for an airline:
- Customers submit applications, airline creates `CLPAccount` contracts
- Uses `key` + `maintainer` to prevent duplicate accounts
- `lookupByKey` checks for existing contracts before creating new ones
- Returns `Optional (ContractId CLPAccount)` — `Some` for new, `None` for duplicates

---

#### `Lab2x.daml` + `TestLab2x.daml`
Extended CLP with:
- `AddPoints` choice for reward point management
- Multi-party testing with Alice and Bob
- Bulk querying using `query @CLPAccount`
- List operations with `DA.List.head`

---

#### `Cresco.daml`
Advanced script using bulk operations and functional patterns:
- `forA` to loop over contract IDs and exercise choices in bulk
- `map`, `zip`, `mapOptional` to transform query results
- Processes all applications in one pass, allocating 1000 points per account

The most advanced script in the repo — production-style bulk contract operations.

---

## Key DAML Concepts Covered

| Concept | Where it appears |
|---|---|
| Functions & type signatures | HelloWorld, Functions |
| Guards, case, if/else | Lab4 |
| Either type & error handling | Lab4 |
| Type classes & instances | Lab3, NewLab6 |
| Optional type | NewLab6, Lab2 |
| map, foldr, foldl | Lab4, Lab5 |
| Tuples & list processing | Lab5 |
| Templates & fields | UserProfile |
| Signatories & observers | Connection, TokenOffer |
| Choices & return types | All contract modules |
| Propose & accept pattern | TokenOffer, MarketPlace |
| Guard conditions (`assertMsg`) | Guardsandcondition |
| Fetching contracts | FetchContract |
| Date & Time types | LoanRequest, Lab2 |
| Dual signatories | LoanRequest, MultiSigTreasury |
| Cross-template creation | CantonOnboarding, MarketPlace |
| DAML Scripts & testing | All test modules |
| Multi-sig authorization | MultiSigTreasury |
| Contract visibility & privacy | MultiSigTreasury |
| Contract keys & `lookupByKey` | Lab2, Lab2x |
| Bulk queries (`query @T`) | Lab2x, Cresco |
| Loops (`forA`) | Cresco |
| List operations (`map`, `zip`) | Cresco |

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
Open any `.daml` file in VS Code, place your cursor inside the script function, and the Script Results panel will show the full transaction trace.

---

## About

Built as part of a structured DAML learning path toward smart contract development on the Canton Network. Covers functional programming foundations through to production-style multi-party smart contracts — with real-world financial use cases including onboarding, lending, marketplace, treasury management, and loyalty programs.