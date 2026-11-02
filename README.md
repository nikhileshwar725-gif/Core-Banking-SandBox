# CoreBanking Sandbox

A toy banking sandbox for FinTech developers. Create sample accounts, transfer money between them with validated rules, and review statements with running balances. Everything runs in the browser.

> **Demo only.** All accounts, customers and amounts are synthetic. No real money moves. This project does not meet banking security, regulatory or accounting standards. Real banking systems need a secure backend, database transactions, audit logs and access control.

## Why this exists

Developers learning FinTech often have no safe place to try basic banking flows. Real bank sandboxes need sign-ups and API keys, and full core-banking stacks are too large to understand quickly. CoreBanking Sandbox keeps the model small so the rules are visible:

- Balances never go negative unless an account has an overdraft flag and stays within its limit.
- Every transfer records a matching debit and credit under the same transfer ID.
- Every statement shows a running balance that ends at the account's closing balance.

## Features

- **Create accounts** with holder name, account type (savings or current), opening deposit and a generated account number.
- **Transfer money** between two different active accounts, with validation and clear reasons when a transfer is blocked.
- **Statements** per account with date, description, debit, credit and running balance, plus opening and closing balances.
- **Overdraft rule** enforced for every transfer. Only the Admin view can set overdraft flags and limits.
- **Admin view** for overdraft settings and resetting sample data (with confirmation).
- **Persistence** in `localStorage` with sample data loaded on first run.
- **Responsive layout** built with Bootstrap 5, usable at phone and laptop widths.

## Business rules

| Rule | Behaviour |
|---|---|
| Amount | Must be greater than 0 |
| Accounts | Source and destination must be different and active |
| Balance | New source balance must be 0 or more, or no lower than minus the overdraft limit when the overdraft flag is on |
| Atomicity | Debit and credit are saved together or not at all |
| Money storage | Amounts stored as whole paise (integers) and displayed with two decimals and the ₹ symbol |

Blocked transfers return a reason and change no balances.

## Tech stack

- HTML5, CSS3, Bootstrap 5
- Vanilla JavaScript (DOM, events, forms, `fetch`, `localStorage`)
- Git and GitHub

Open `index.html` in a browser. On first load, the app seeds sample data.

## Usage

1. **Accounts:** Open Accounts and select **Create Account**. Enter the holder name, account type and opening deposit.
2. **Transfer:** Open Transfer, choose a source and destination, enter an amount and an optional reference, then submit.
3. **Statement:** Open Statement, pick an account, and check the opening balance, each row's running balance and the closing balance. Filter by date or type, or export to CSV.
4. **Admin view:** Switch roles to set overdraft flags and limits, or reset sample data.
5. # developer view
6. 

## Sample data

On first load the sandbox seeds:

- Five synthetic accounts, one with an overdraft flag
- About 20 transactions so every screen has data

Use the Admin view's reset control to restore the documented sample data.

## Data model

| Key | Contents |
|---|---|
| `corebank_accounts` | Account records (ID, number, holder, type, opening deposit, overdraft flag and limit, status, created date) |
| `corebank_transactions` | Ledger entries (transaction ID, transfer ID, account, debit or credit, amount in paise, reference, balance after) |
| `corebank_transfers` | Transfer attempts and their results (status, reason) |
| `corebank_prefs` | Non-sensitive UI choices (role, last account, statement filters) |

## Testing

Test cases cover:

- Valid and invalid amounts (zero, negative, non-numeric)
- Same-account transfers
- Transfers that would overdraw, with and without an overdraft flag
- Transfers exactly at the overdraft limit']\
- Statement closing balance equal to account balance

Add the test command once tests are in place.

## Accessibility

- Every field has a visible label.
- Debits and credits appear in separate labelled columns, not by colour alone.
- Transfer results are announced with `aria-live`.
- Focus indicators are visible, and the app is keyboard-navigable.
- Text colours meet WCAG AA contrast (for example, body text `#1F2937` on white is 14.7:1).

## Deployment

The app is static and is hosted on GitHub Pages.

Settings → Pages → deploy from the `main` branch root.

Live demo: `<add link once deployed>`

## Screenshots

Add screenshots at phone and laptop widths:

| Laptop | Phone |
|---|---|
| `docs/laptop-dashboard.png` | `docs/phone-transfer.png` |

## Project structure

Planned layout (update to match the final repo):

```
corebank-sandbox/
├── index.html          # App shell and screens
├── css/
│   └── styles.css      # Custom styles on top of Bootstrap 5
├── js/
│   ├── app.js          # UI wiring and events
│   ├── transfer.js     # TransferService: transfer rules
│   ├── balance.js      # BalanceCalculator: balances and running balances
│   └── storage.js      # LocalDataStore: load, save, reset
├── tests/              # Test cases
├── docs/               # Screenshots and wireframes
└── README.md
```

## Limitations

This is a learning sandbox. Do not use it for real money or real customer data.

- Rules run on the client. There is no server-side validation.
- There is no login, authentication or authorisation. The Admin view is a UI switch, not a security boundary.
- `localStorage` can be edited or cleared by anyone with access to the browser.
- There is no database transaction layer, so atomicity is approximated in code.
- Out of scope: real payment rails (UPI, NEFT, card networks), bank connections, KYC, AML checks, interest, fees, loans, and multi-currency support.

A production system would need a secure backend, database transactions, audit trails, encryption, and compliance with the relevant regulations.

## Roadmap

- [ ] Hash-chained ledger to detect tampering with past entries
- [ ] Balance chart with Chart.js
- [ ] React + Vite interface
- [ ] Search and filters on accounts and statements

## Contributing

Issues and pull requests are welcome. Keep commits small and describe the rule or screen you changed.

## Acknowledgements

Built as a 4-week OJT project (PST_PD_153, Polaris School of Technology, 2026 Level 2).

**Team:** Kunal Tayde, Gajangi Nikhileshwar
**Mentor / Reviewer:** Madhushri Tangali

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
