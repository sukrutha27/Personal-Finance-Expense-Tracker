# 💰 Personal Finance Tracker

A desktop personal finance management application built with Python, Tkinter, and SQLite. Track your income and expenses,
set savings goals,manage category budgets, and visualize your spending habits with interactive charts.
---
## 📸 Features

- 🔐 **User Authentication** — Register and log in with secure credentials
- ➕ **Transaction Management** — Add and view income/expense transactions by date and category
- 🎯 **Savings Goals** — Set a savings target and track your progress
- 📥 **Savings Updates** — Incrementally log amounts saved over time
- 💰 **Category Budgets** — Set spending limits per category
- 🚨 **Budget Alerts** — Get warnings when you exceed a category budget
- 📊 **Bar Chart** — Visualize expenses by category
- 🥧 **Pie Chart** — See your expense distribution at a glance

---

## 🛠️ Tech Stack

| Layer       | Technology         |
|-------------|--------------------|
| Language    | Python 3.x         |
| GUI         | Tkinter            |
| Database    | SQLite3            |
| Charts      | Matplotlib         |

---

## 📁 Project Structure

```
finance-tracker/
│
├── finance_tracker.py      # Main application file
├── finance_tracker.db      # SQLite database (auto-created on first run)
└── README.md
```

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/finance-tracker.git
cd finance-tracker
```

### 2. Install Dependencies

Make sure you have Python 3 installed, then install required packages:

```bash
pip install matplotlib
```

> `tkinter` and `sqlite3` come bundled with standard Python installations.

### 3. Run the Application

```bash
python finance_tracker.py
```

---

## 🚀 Usage

### Register & Login
- Launch the app and click **Register** to create a new account
- Log in with your credentials to access your personal dashboard

### Adding Transactions
- Click **➕ Add Transaction**
- Enter the **Date** (e.g., `2024-01-15`), **Category** (e.g., `Food`), **Amount**, and **Type** (`Income` or `Expense`)

### Setting a Savings Goal
- Click **🎯 Set Savings Goal** and enter your target amount
- Use **📥 Update Savings** to log savings over time
- Check **📊 Check Savings Progress** to see how close you are to your goal

### Budget Management
- Click **💰 Set Category Budget** to define a spending limit for any category
- Click **🚨 Check Budget Alerts** to see if you've gone over budget in any category

### Charts
- **📊 View Expense Bar Chart** — Bar graph of spending per category
- **🥧 View Expense Pie Chart** — Pie chart showing expense distribution

---

## 🗃️ Database Schema

```sql
users (id, username, password)
transactions (id, user_id, date, category, amount, type)
savings (user_id, goal, saved)
category_budgets (user_id, category, budget)
```

---

## 📌 Notes

- Passwords are stored in plaintext in the local SQLite database. This app is intended for **personal/local use only**.
- The database file `finance_tracker.db` is auto-generated in the same directory on first run.

---

## 🙌 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
