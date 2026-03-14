<div align="center">

# fintrack

**Terminal-based personal finance tracker**
*Budget smarter. Spend mindfully. Save effortlessly.*

![Python](https://img.shields.io/badge/Python-3.6+-black?style=flat-square)
![Dependencies](https://img.shields.io/badge/Dependencies-None-black?style=flat-square)

</div>

---

## Overview

**fintrack** is a clean, distraction-free CLI tool for managing your personal finances. No sign-ups. No dashboards. No noise — just your money, clearly laid out in the terminal.

---

## Getting Started

```bash
# Clone the repo
git clone https://github.com/your-username/fintrack.git
cd fintrack

# Run
python finance_tracker.py
```

---

## Features

```
  Set Budget          →   Define your monthly spending ceiling
  Log Income          →   Record earnings with category & date
  Log Expenses        →   Track every outgoing with full context
  Live Summary        →   Balance snapshot + visual budget bar
  Transaction Log     →   Timestamped history of all entries
  Reports             →   Daily, weekly, or monthly breakdowns
  Auto-save           →   Persists to local JSON automatically
```

---

## Interface

```
Personal Finance Tracker
─────────────────────────
1.  Set Monthly Budget
2.  Add Income
3.  Add Expense
4.  Show Summary
5.  Show All Transactions
6.  Generate Report
7.  Exit
─────────────────────────
Select an option (1–7):
```

---

## Summary View

```
─── Summary ───────────────────────────────────
  Budget this month   $2,000.00
  Total income        $3,500.00
  Total expenses      $1,200.00
  Balance             $2,300.00

  Budget used   |████████████████──────────|  60.0%
───────────────────────────────────────────────
```

---

## Data

All data is stored locally in `finance_data.json`. No cloud, no accounts — just a file on your machine.

```json
{
    "budget": 2000.00,
    "transactions": [
        {
            "type": "income",
            "category": "Freelance",
            "amount": 1500.00,
            "timestamp": "2026-03-14"
        }
    ]
}
```

---

## Project Structure

```
fintrack/
├── finance_tracker.py    — core application
├── finance_data.json     — local data store (auto-generated)
└── README.md
```

---

## Limitations

- Local storage only — no sync or multi-device support
- Single user — no authentication
- USD formatting only

---

<div align="center">

MIT License · Made with Python · No dependencies

</div>
