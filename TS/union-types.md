# Union Types

```ts
const LoanPurposes = ["Purchase", "Refinance", "MortgageFirst"] as const; //readonly

type LoanPurposeTuple = typeof LoanPurposes;

type LoanPurposeUnion = LoanPurposeTuple[number]; //Purchase | Refinance | MortgageFirst
```