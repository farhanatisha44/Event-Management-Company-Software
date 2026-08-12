# Event Management Company Software

A C++ console application modeling the internal staff structure, payroll, and profitability of an event management company, built around an abstract base class and a deep inheritance hierarchy covering every role — from event managers and photographers to caterers, decorators, and beauticians.

## Overview

The program demonstrates object-oriented design principles (abstract base classes, single and multiple inheritance, friend functions) by modeling a small event company's staff and finances:

- Each **employee role** is its own class, capturing that role's salary and personal details.
- All roles ultimately derive from a shared **`CEO`** abstract base class, which defines the common interface (`get_data()`, `calculate_salary()`, `show_data()`) every role must implement.
- An **`Events`** class tracks event bookings — client details, event type, and budget.
- A **`Balance_sheet`** class (via multiple inheritance from every staff role) aggregates all costs.
- A free `calculate_profit()` function, granted `friend` access into each class, computes the company's yearly profit by comparing total event revenue against total staff costs.

## Class Hierarchy

```
CEO (abstract base — pure virtual get_data/calculate_salary/show_data)
└── Event_manager
    ├── Photography
    │   ├── Videographer
    │   ├── Editor
    │   └── Cameraman
    ├── Decorator
    ├── Caterer
    ├── Product_supplier
    ├── Staff
    ├── Beautician
    └── Events

Balance_sheet
  (multiply inherits from Videographer, Cameraman, Editor,
   Decorator, Caterer, Beautician, Product_supplier, Staff)
```

| Class | Represents |
|---|---|
| `CEO` | Abstract interface all roles implement |
| `Event_manager` | Base employee role — name, account number, salary; parent of most other roles |
| `Photography` | Empty intermediate class grouping the three photography-related roles |
| `Videographer`, `Editor`, `Cameraman` | Photography team roles, each with their own salary field |
| `Decorator`, `Caterer`, `Product_supplier`, `Staff`, `Beautician` | Other operational roles, each with their own salary field |
| `Events` | Tracks event bookings — event number, type, budget, and client contact details |
| `Balance_sheet` | Aggregates "other costs" and, via multiple inheritance, has access to every role's salary data for profit calculation |

## Salary Calculation

Each role's `calculate_salary()` prompts for a base salary and adds a small fixed benefits addition:
- **Event Manager:** base salary + 0.02 (home loan benefit) + 0.025 (medical benefit)
- **All other roles:** base salary + 0.025 (medical benefit)

> Note: these benefit additions are literal constants (e.g. `+0.025`) rather than percentages — see [Known Issues](#known-issues) below.

## Profit Calculation

`calculate_profit()` takes one instance of every class and computes:

```
total_spending = other_costs + (each role's salary × 12)
remainder = event_budget − total_spending
```

This gives a rough yearly profit/loss figure by comparing total annual event revenue against total annual staff costs plus other overhead.

## Files

| File | Description |
|---|---|
| `project.cpp` | The main/current version of the program (402 lines) — full class hierarchy, profit calculation, and driver `main()` |
| `1.cpp` | An earlier draft of the same program (236 lines, lowercase class names, uses `conio.h`) — kept for reference/history |
| `project.o` | Compiled object file from a previous build |

## How to Build & Run

```bash
g++ project.cpp -o event_software
./event_software
```

The program will interactively prompt you, in order, for:
1. Event manager's name, account number, and salary
2. Videographer, Cameraman, Editor, Beautician, Caterer, Staff, Decorator, and Product Supplier details and salaries
3. Event details (event number `-1` ends event entry, budget, type, client name/phone/address)
4. Balance sheet's "other costs"

It then prints the computed total cost/profit figure at the end.
