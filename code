import sqlite3
import matplotlib.pyplot as plt
import tkinter as tk
from tkinter import ttk, messagebox

# 📌 Database Connection
conn = sqlite3.connect("finance_tracker.db")
cursor = conn.cursor()

# 📌 Create Tables
cursor.execute('''CREATE TABLE IF NOT EXISTS users (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    username TEXT UNIQUE,
                    password TEXT)''')

cursor.execute('''CREATE TABLE IF NOT EXISTS transactions (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    user_id INTEGER,
                    date TEXT,
                    category TEXT,
                    amount REAL,
                    type TEXT,
                    FOREIGN KEY(user_id) REFERENCES users(id))''')

cursor.execute('''CREATE TABLE IF NOT EXISTS savings (
                    user_id INTEGER PRIMARY KEY,
                    goal REAL DEFAULT 0,
                    saved REAL DEFAULT 0,
                    FOREIGN KEY(user_id) REFERENCES users(id))''')

cursor.execute('''CREATE TABLE IF NOT EXISTS category_budgets (
                    user_id INTEGER,
                    category TEXT,
                    budget REAL,
                    PRIMARY KEY(user_id, category),
                    FOREIGN KEY(user_id) REFERENCES users(id))''')

conn.commit()

# 📌 Main App Class
class FinanceApp:
    def __init__(self, root):
        self.root = root
        self.root.title("💰 Personal Finance Tracker")
        self.root.geometry("600x500")
        self.root.configure(bg="#f4f4f4")  # Light background for the main window
        self.user_id = None

        self.main_frame = tk.Frame(self.root, bg="#f4f4f4")  # Light background for the main frame
        self.main_frame.pack(expand=True, fill="both")

        self.show_login_screen()

    def switch_frame(self, new_frame_func):
        """Destroy existing widgets and load a new frame."""
        for widget in self.main_frame.winfo_children():
            widget.destroy()
        new_frame_func()

    def show_login_screen(self):
        self.switch_frame(self.create_login_screen)

    def create_login_screen(self):
        tk.Label(self.main_frame, text="📋 Enter Username:", bg="#f4f4f4", fg="#2b5797", font=("Arial", 12)).pack(pady=5)
        username_entry = tk.Entry(self.main_frame, width=30)
        username_entry.pack(pady=5)

        tk.Label(self.main_frame, text="🔐 Enter Password:", bg="#f4f4f4", fg="#2b5797", font=("Arial", 12)).pack(pady=5)
        password_entry = tk.Entry(self.main_frame, width=30, show="*")
        password_entry.pack(pady=5)

        def login():
            username = username_entry.get()
            password = password_entry.get()
            cursor.execute("SELECT id FROM users WHERE username=? AND password=?", (username, password))
            user = cursor.fetchone()
            if user:
                self.user_id = user[0]
                self.initialize_savings()
                self.show_main_menu()
            else:
                messagebox.showerror("Error", "Invalid credentials")

        tk.Button(self.main_frame, text="Login", command=login, bg="#4CAF50", fg="white", font=("Arial", 10),
                  relief="flat", width=15).pack(pady=10)
        tk.Button(self.main_frame, text="Register", command=self.show_registration_screen, bg="#0078D7", fg="white",
                  font=("Arial", 10), relief="flat", width=15).pack(pady=5)

    def show_registration_screen(self):
        self.switch_frame(self.create_registration_screen)

    def create_registration_screen(self):
        tk.Label(self.main_frame, text="📝 Create a New Account", bg="#f4f4f4", fg="#0078D7", font=("Arial", 14)).pack(pady=10)

        tk.Label(self.main_frame, text="📋 Enter Username:", bg="#f4f4f4", fg="#2b5797", font=("Arial", 12)).pack(pady=5)
        username_entry = tk.Entry(self.main_frame, width=30)
        username_entry.pack(pady=5)

        tk.Label(self.main_frame, text="🔐 Enter Password:", bg="#f4f4f4", fg="#2b5797", font=("Arial", 12)).pack(pady=5)
        password_entry = tk.Entry(self.main_frame, width=30, show="*")
        password_entry.pack(pady=5)

        tk.Label(self.main_frame, text="🔐 Confirm Password:", bg="#f4f4f4", fg="#2b5797", font=("Arial", 12)).pack(pady=5)
        confirm_password_entry = tk.Entry(self.main_frame, width=30, show="*")
        confirm_password_entry.pack(pady=5)

        def register():
            username = username_entry.get()
            password = password_entry.get()
            confirm_password = confirm_password_entry.get()

            if password != confirm_password:
                messagebox.showerror("Error", "Passwords do not match")
                return

            try:
                cursor.execute("INSERT INTO users (username, password) VALUES (?, ?)", (username, password))
                conn.commit()
                messagebox.showinfo("Success", "Account Created!")
                self.show_login_screen()
            except sqlite3.IntegrityError:
                messagebox.showerror("Error", "Username already exists")

        tk.Button(self.main_frame, text="Register", command=register, bg="#4CAF50", fg="white", font=("Arial", 10),
                  relief="flat", width=15).pack(pady=10)
        tk.Button(self.main_frame, text="Back to Login", command=self.show_login_screen, bg="#0078D7", fg="white",
                  font=("Arial", 10), relief="flat", width=15).pack(pady=5)

    def initialize_savings(self):
        """Ensure the user has a savings entry."""
        cursor.execute("INSERT OR IGNORE INTO savings (user_id) VALUES (?)", (self.user_id,))
        conn.commit()

    def show_main_menu(self):
        """Display main menu options."""
        self.switch_frame(lambda: None)
        tk.Label(self.main_frame, text="Main Menu", bg="#f4f4f4", fg="#0078D7", font=("Arial", 16)).pack(pady=10)

        options = [
            ("➕ Add Transaction", self.add_transaction, "#FFC107"),
            ("📜 View Transactions", self.view_transactions, "#FFC107"),
            ("🎯 Set Savings Goal", self.set_savings_goal, "#4CAF50"),
            ("📥 Update Savings", self.update_savings, "#4CAF50"),
            ("📊 Check Savings Progress", self.check_savings_progress, "#0078D7"),
            ("💰 Set Category Budget", self.set_category_budget, "#0078D7"),
            ("🚨 Check Budget Alerts", self.check_budget_alerts, "#F44336"),
            ("📊 View Expense Bar Chart", self.view_expense_bar_chart, "#FFC107"),
            ("🥧 View Expense Pie Chart", self.view_expense_pie_chart, "#FFC107"),
            ("❌ Exit", self.root.quit, "#F44336")
        ]

        for text, command, color in options:
            tk.Button(self.main_frame, text=text, command=command, bg=color, fg="white", font=("Arial", 12),
                      relief="flat", width=40).pack(pady=5)

    # Add methods like add_transaction, view_transactions, set_savings_goal, etc.
    def add_transaction(self):
        def save_transaction(values):
            cursor.execute(
                "INSERT INTO transactions (user_id, date, category, amount, type) VALUES (?, ?, ?, ?, ?)",
                (self.user_id, values["Date"], values["Category"], float(values["Amount"]), values["Type"]))
            conn.commit()

            # Check if transaction exceeds budget
            cursor.execute("SELECT budget FROM category_budgets WHERE user_id=? AND category=?",
                           (self.user_id, values["Category"]))
            budget = cursor.fetchone()
            if budget:
                budget_amount = budget[0]
                cursor.execute(
                    "SELECT SUM(amount) FROM transactions WHERE user_id=? AND category=? AND type='Expense'",
                    (self.user_id, values["Category"]))
                total_expenses = cursor.fetchone()[0] or 0
                if total_expenses > budget_amount:
                    messagebox.showwarning(
                        "Budget Alert",
                        f"Category '{values['Category']}' exceeded its budget!\nBudget: ${budget_amount}\nExpenses: ${total_expenses}"
                    )

            messagebox.showinfo("Success", "Transaction Added!")
            self.view_transactions()

        self.create_form("➕ Add Transaction", ["Date", "Category", "Amount", "Type"], save_transaction)

    def create_form(self, title, fields, submit_action):
        """Creates a form with given fields and action."""
        self.switch_frame(lambda: None)

        tk.Label(self.main_frame, text=title, font=("Arial", 14)).pack(pady=10)
        entries = {}

        for field in fields:
            tk.Label(self.main_frame, text=field).pack(pady=5)
            entry = tk.Entry(self.main_frame, width=30)
            entry.pack(pady=5)
            entries[field] = entry

        def submit():
            values = {field: entry.get() for field, entry in entries.items()}
            submit_action(values)

        tk.Button(self.main_frame, text="Submit", command=submit).pack(pady=10)
        tk.Button(self.main_frame, text="Back", command=self.show_main_menu).pack(pady=5)

    def set_category_budget(self):
        def save_category_budget(values):
            cursor.execute(
                "INSERT OR REPLACE INTO category_budgets (user_id, category, budget) VALUES (?, ?, ?)",
                (self.user_id, values["Category"], float(values["Budget Amount"])))
            conn.commit()
            messagebox.showinfo("Success", "Category Budget Set!")
            self.show_main_menu()

        self.create_form("💰 Set Category Budget", ["Category", "Budget Amount"], save_category_budget)

    def view_transactions(self):
        self.display_table("SELECT date, category, amount, type FROM transactions WHERE user_id=?",
                           "📜 Transactions")

    def display_table(self, query, title):
        self.switch_frame(lambda: None)

        tk.Label(self.main_frame, text=title, font=("Arial", 14)).pack(pady=10)

        columns = ["Date", "Category", "Amount", "Type"]
        tree = ttk.Treeview(self.main_frame, columns=columns, show="headings")

        for col in columns:
            tree.heading(col, text=col)
            tree.column(col, anchor="center")

        tree.pack(expand=True, fill="both", pady=10)

        cursor.execute(query, (self.user_id,))
        for row in cursor.fetchall():
            tree.insert("", "end", values=row)

        tk.Button(self.main_frame, text="Back", command=self.show_main_menu).pack(pady=10)

    def set_savings_goal(self):
        def save_goal(values):
            cursor.execute("UPDATE savings SET goal=? WHERE user_id=?",
                           (float(values["Goal Amount"]), self.user_id))
            conn.commit()
            messagebox.showinfo("Success", "Savings Goal Set!")
            self.show_main_menu()

        self.create_form("🎯 Set Savings Goal", ["Goal Amount"], save_goal)

    def update_savings(self):
        """Update the savings amount for the user."""

        def save_savings(values):
            cursor.execute("UPDATE savings SET saved=saved+? WHERE user_id=?",
                           (float(values["Amount Saved"]), self.user_id))
            conn.commit()
            messagebox.showinfo("Success", "Savings Updated!")
            self.show_main_menu()

        self.create_form("📥 Update Savings", ["Amount Saved"], save_savings)

    def check_savings_progress(self):
        """Check the user's progress towards their savings goal."""
        cursor.execute("SELECT goal, saved FROM savings WHERE user_id=?", (self.user_id,))
        goal, saved = cursor.fetchone()
        progress = (saved / goal) * 100 if goal > 0 else 0

        messagebox.showinfo("Savings Progress", f"Goal: ${goal}\nSaved: ${saved}\nProgress: {progress:.2f}%")

    def check_budget_alerts(self):
        """Check and display all budget alerts."""
        cursor.execute("SELECT category, budget FROM category_budgets WHERE user_id=?", (self.user_id,))
        budgets = cursor.fetchall()

        alerts = []
        for category, budget in budgets:
            cursor.execute("SELECT SUM(amount) FROM transactions WHERE user_id=? AND category=? AND type='Expense'",
                           (self.user_id, category))
            total_expenses = cursor.fetchone()[0] or 0
            if total_expenses > budget:
                alerts.append(
                    f"Category '{category}' exceeded its budget!\nBudget: ${budget}\nExpenses: ${total_expenses}")

        if alerts:
            messagebox.showwarning("Budget Alerts", "\n\n".join(alerts))
        else:
            messagebox.showinfo("Budget Alerts", "You're within all category budgets! 🎉")

    def view_expense_bar_chart(self):
        """Display a bar chart of expenses by category."""
        cursor.execute(
            "SELECT category, SUM(amount) FROM transactions WHERE user_id=? AND type='Expense' GROUP BY category",
            (self.user_id,))
        data = cursor.fetchall()

        if not data:
            messagebox.showinfo("No Data", "No expense data available.")
            return

        categories, amounts = zip(*data)

        plt.figure(figsize=(8, 5))
        plt.bar(categories, amounts, color='skyblue')
        plt.xlabel("Categories")
        plt.ylabel("Amount Spent ($)")
        plt.title("💰 Expenses by Category")
        plt.xticks(rotation=45)
        plt.show()

    def view_expense_pie_chart(self):
        """Display a pie chart of expenses by category."""
        cursor.execute(
            "SELECT category, SUM(amount) FROM transactions WHERE user_id=? AND type='Expense' GROUP BY category",
            (self.user_id,))
        data = cursor.fetchall()

        if not data:
            messagebox.showinfo("No Data", "No expense data available.")
            return

        categories, amounts = zip(*data)

        plt.figure(figsize=(7, 7))
        plt.pie(amounts, labels=categories, autopct="%1.1f%%", startangle=140)
        plt.title("🥧 Expense Distribution")
        plt.show()
# 📌 Run Application
if __name__ == "__main__":
    root = tk.Tk()
    app = FinanceApp(root)
    root.mainloop()
