import json
import os
from datetime import datetime, timedelta

DATA_FILE = "finance_data.json"

def load_data():
    if not os.path.exists(DATA_FILE):
        return {"budget": 0.0, "transactions": []}
    try:
        with open(DATA_FILE, "r") as f:
            return json.load(f)
    except (json.JSONDecodeError, IOError):
        print("Warning: Failed to load saved data, starting fresh.")
        return {"budget": 0.0, "transactions": []}

def save_data(data):
    try:
        with open(DATA_FILE, "w") as f:
            json.dump(data, f, indent=4)
    except IOError:
        print("Error: Could not save data to file.")

def format_currency(amount):
    return f"${amount:.2f}"

def show_summary(data):
    income = sum(t['amount'] for t in data['transactions'] if t['type'] == 'income')
    expenses = sum(t['amount'] for t in data['transactions'] if t['type'] == 'expense')
    balance = income - expenses
    print("\n--- Summary ---")
    print(f"Budget this month: {format_currency(data['budget'])}")
    print(f"Total income   : {format_currency(income)}")
    print(f"Total expenses : {format_currency(expenses)}")
    print(f"Balance        : {format_currency(balance)}")
    if data['budget'] > 0:
        percent_used = (expenses / data['budget']) * 100
        percent_used = min(percent_used, 100)
        bar_length = 30
        filled_length = int(bar_length * percent_used // 100)
        bar = '█' * filled_length + '-' * (bar_length - filled_length)
        print(f"Budget used   : |{bar}| {percent_used:.1f}%")
        if expenses > data['budget']:
            print("Warning: You have exceeded your budget!")
    print("----------------\n")

def show_transactions(data):
    if not data['transactions']:
        print("No transactions recorded yet.")
        return
    print("\n--- Transactions ---")
    for idx, t in enumerate(data['transactions'], start=1):
        time_stamp = t.get("timestamp", "")
        ts_str = f"[{time_stamp}]" if time_stamp else ""
        print(f"{idx}. {ts_str} {t['type'].capitalize():7} | {t['category'][:30]:30} | {format_currency(t['amount'])}")
    print("--------------------\n")

def add_transaction(data, trans_type):
    desc = input(f"Enter {trans_type} category: ").strip()
    if not desc:
        print("Category cannot be empty.")
        return
    try:
        amt = float(input(f"Enter {trans_type} amount: "))
        if amt <= 0:
            print("Amount must be positive.")
            return
    except ValueError:
        print("Invalid amount entered.")
        return
    date_str = input("Enter date (YYYY-MM-DD) or press Enter for today: ").strip()
    if not date_str:
        date_str = datetime.now().strftime("%Y-%m-%d")
    try:
        date = datetime.strptime(date_str, "%Y-%m-%d")
    except ValueError:
        print("Invalid date format. Please use YYYY-MM-DD.")
        return
    transaction = {
        "type": trans_type,
        "category": desc,
        "amount": amt,
        "timestamp": date.strftime("%Y-%m-%d")
    }
    data['transactions'].append(transaction)
    save_data(data)
    print(f"{trans_type.capitalize()} added successfully!")

def set_budget(data):
    try:
        b = float(input("Enter your monthly budget amount: "))
        if b < 0:
            print("Budget cannot be negative.")
            return
        data['budget'] = b
        save_data(data)
        print("Budget updated.")
    except ValueError:
        print("Invalid budget amount.")

def get_transactions_in_period(data, start_date, end_date):
    filtered = []
    for t in data['transactions']:
        t_date = datetime.strptime(t['timestamp'], "%Y-%m-%d")
        if start_date <= t_date <= end_date:
            filtered.append(t)
    return filtered

def generate_report(data):
    print("Select report period:")
    print("1. Daily")
    print("2. Weekly")
    print("3. Monthly")
    choice = input("Enter choice (1-3): ").strip()
    today = datetime.now()
    if choice == '1':  # Daily
        start = today.replace(hour=0, minute=0, second=0, microsecond=0)
        end = start + timedelta(days=1) - timedelta(seconds=1)
        period_str = start.strftime("%Y-%m-%d")
    elif choice == '2':  # Weekly: Last 7 days (including today)
        end = today.replace(hour=23, minute=59, second=59, microsecond=999999)
        start = end - timedelta(days=6)
        period_str = f"{start.strftime('%Y-%m-%d')} to {end.strftime('%Y-%m-%d')}"
    elif choice == '3':  # Monthly: current month
        start = today.replace(day=1, hour=0, minute=0, second=0, microsecond=0)
        next_month = (start.replace(day=28) + timedelta(days=4)).replace(day=1)
        end = next_month - timedelta(seconds=1)
        period_str = start.strftime("%Y-%m")
    else:
        print("Invalid choice for report period.")
        return

    filtered = get_transactions_in_period(data, start, end)

    income = sum(t['amount'] for t in filtered if t['type'] == 'income')
    expenses = sum(t['amount'] for t in filtered if t['type'] == 'expense')
    balance = income - expenses

    print(f"\n=== Report for {period_str} ===")
    print(f"Total Income   : {format_currency(income)}")
    print(f"Total Expenses : {format_currency(expenses)}")
    print(f"Balance        : {format_currency(balance)}")

    # Remaining budget for the period (if monthly)
    if choice == '3':
        budget_monthly = data['budget']
        remaining_budget = budget_monthly - expenses
        print(f"Monthly Budget : {format_currency(budget_monthly)}")
        print(f"Remaining Budget: {format_currency(remaining_budget)}")
        if remaining_budget < 0:
            print("Warning: You have exceeded your budget this month!")
    print("==============================\n")

def main_menu():
    print("Personal Finance Tracker")
    print("1. Set Monthly Budget")
    print("2. Add Income")
    print("3. Add Expense")
    print("4. Show Summary")
    print("5. Show All Transactions")
    print("6. Generate Report")
    print("7. Exit")

def main():
    data = load_data()
    while True:
        main_menu()
        choice = input("Select an option (1-7): ").strip()
        if choice == '1':
            set_budget(data)
        elif choice == '2':
            add_transaction(data, "income")
        elif choice == '3':
            add_transaction(data, "expense")
        elif choice == '4':
            show_summary(data)
        elif choice == '5':
            show_transactions(data)
        elif choice == '6':
            generate_report(data)
        elif choice == '7':
            print("Exiting. Have a nice day!")
            break
        else:
            print("Invalid choice. Please select a valid option.")

if __name__ == "__main__":
    main()
