# python-project
This the python project(name- expenditure tracker of the family)
#intputing the salary of the mebers of the family:
def input_family_salary(users, username):
    if username in users:
        user_data = users[username]
        num_family_members = int(input("Enter the number of family members: "))
        for _ in range(num_family_members):
            name = input("Enter family member's name: ")
            salary = float(input(f"Enter salary for {name}: "))
            user_data['family_salaries'][name] = salary
        print("Family salaries added successfully!")
    else:
        print("User does not exist!")
# user_management
#this function is for opennig the file to add the information of the user
def load_users():
    users = {}
    try:
        with open('users.txt', 'r') as file:
            for line in file:
                username, data = line.strip().split(':')
                user_data = eval(data)
                users[username] = user_data
    except FileNotFoundError:
        pass
    return users
#this function is used saving the details of the user
def save_users(users):
    with open('users.txt', 'w') as file:
        for username, user_data in users.items():
            file.write(f"{username}:{user_data}\n")
#this function is used to add the user
def add_user(users, username):
    if username not in users:
        users[username] = {
            'family_expenses': {},
            'daily_expenses': {},
            'total_income': 0,
            'family_salaries': {}
        }
        print(f"User '{username}' added successfully!")
        set_user_income(users, username)
        input_family_salary(users, username)
        add_family_expense(users, username)
        add_daily_expense(users, username)
    else:
        print("User already exists!")
#for setting the totall income
def set_user_income(users, username):
    income = float(input(f"Enter total income for user '{username}': "))
    users[username]['total_income'] = income
    print(f"Income set successfully for user '{username}'!")
#for asking different income
def add_family_expense(users, username):
    num_family_members = int(input("Enter the number of family members for family expenses: "))
    for _ in range(num_family_members):
        name = input("Enter family member's name: ")
        amount = float(input(f"Enter expense amount for {name}: "))
        users[username]['family_expenses'][name] = amount
    print("Family expenses added successfully!")
#for knowing the expenditure
def add_daily_expense(users, username):
    if username in users:
        user_data = users[username]
        while True:
            print("\nSelect daily expense category:")
            print("1. Household")
            print("2. Rent")
            print("3. Electrical")
            print("4. Others")
            print("5. Exit")

            choice = input("Enter your choice: ")

            if choice == '1':
                category = "Household"
                break
            elif choice == '2':
                category = "Rent"
                break
            elif choice == '3':
                category = "Electrical"
                break
            elif choice == '4':
                category = "Others"
                break
            elif choice == '5':
                print("Exiting...")
                return
            else:
                print("Invalid choice! Please enter a valid option.")

        amount = float(input(f"Enter expense amount for {category}: "))
        user_data['daily_expenses'][category] = amount
        print("Daily expenses added successfully!")
    else:
        print("User does not exist!")
def add_expenditure(users, username):
    if username in users:
        user_data = users[username]
        print("\nSelect an expenditure category:")
        print("1. Family Expenses")
        print("2. Daily Expenses")
        print("3. Go back")

        choice = input("Enter your choice: ")

        if choice == '1':
            add_family_expense(users, username)
        elif choice == '2':
            add_daily_expense(users, username)
        elif choice == '3':
            print("Going back to main menu...")
        else:
            print("Invalid choice! Please enter a valid option.")

    else:
        print("User does not exist!")


def edit_expenditure(users, username):
    if username in users:
        user_data = users[username]
        print("\nSelect an expenditure category to edit:")
        print("1. Family Expenses")
        print("2. Daily Expenses")
        print("3. Go back")

        choice = input("Enter your choice: ")

        if choice == '1':
            edit_family_expense(users, username)
        elif choice == '2':
            edit_daily_expense(users, username)
        elif choice == '3':
            print("Going back to main menu...")
        else:
            print("Invalid choice! Please enter a valid option.")

    else:
        print("User does not exist!")


def view_expenditure(users, username):
    if username in users:
        user_data = users[username]
        print("\nSelect an expenditure category to view:")
        print("1. Family Expenses")
        print("2. Daily Expenses")
        print("3. Go back")

        choice = input("Enter your choice: ")

        if choice == '1':
            view_family_expenses(users, username)
        elif choice == '2':
            view_daily_expenses(users, username)
        elif choice == '3':
            print("Going back to main menu...")
        else:
            print("Invalid choice! Please enter a valid option.")

    else:
        print("User does not exist!")


#selecting the user
def select_user(users, username):
    if username in users:
        print(f"User '{username}' selected.")
        return username
    else:
        print("User does not exist!")
        return None
#editiing the user details
def edit_user_details(users, username):
    if username in users:
        user_data = users[username]
        print(f"Editing details for user '{username}':")
        choice = input("Do you want to edit income? (yes/no): ").lower()
        if choice == 'yes':
            set_user_income(users, username)
        choice = input("Do you want to add family members? (yes/no): ").lower()
        if choice == 'yes':
            add_family_expense(users, username)
        choice = input("Do you want to edit family expenses? (yes/no): ").lower()
        if choice == 'yes':
            edit_family_expense(users, username)
        choice = input("Do you want to edit daily expenses? (yes/no): ").lower()
        if choice == 'yes':
            edit_daily_expense(users, username)
    else:
        print("User does not exist!")

def edit_income(users, username, new_income):
    if username in users:
        users[username]['total_income'] = new_income
        print("Income edited successfully!")
    else:
        print("User does not exist!")

def edit_family_expense(users, username):
    if username in users:
        user_data = users[username]
        for member in user_data['family_expenses']:
            new_amount = float(input(f"Enter new expense amount for {member}: "))
            user_data['family_expenses'][member] = new_amount
        print("Family Expenses edited successfully!")
    else:
        print("User does not exist!")



def edit_daily_expense(users, username):
    if username in users:
        user_data = users[username]
        for category in user_data['daily_expenses']:
            new_amount = float(input(f"Enter new expense amount for {category}: "))
            user_data['daily_expenses'][category] = new_amount
        print("Daily Expenses edited successfully!")
    else:
        print("User does not exist!")



def calculate_budget(users, username):
    if username in users:
        user_data = users[username]
        total_expenses = sum(user_data['family_expenses'].values()) + sum(user_data['daily_expenses'].values())
        budget = user_data['total_income'] - total_expenses
        return budget
    else:
        print("No user selected!")



def view_income(users, username):
    if username in users:
        print(f"Total Income for user '{username}': ${users[username]['total_income']}")
    else:
        print("No user selected!")


def view_budget(users, username):
    if username in users:
        budget = calculate_budget(users, username)
        if budget is not None:
            if budget >= 0:
                print(f"Remaining budget for user '{username}': ${budget}")
            else:
                print(f"User '{username}' has exceeded the budget by ${-budget}")
        else:
            print("Budget calculation failed!")
    else:
        print("No user selected!")

def view_family_expenses(users, username):
    if username in users:
        user_data = users[username]
        print(f"Family Expenses for user '{username}':")
        for member, expense in user_data['family_expenses'].items():
            print(f"{member}: ${expense}")
    else:
        print("No user selected!")

def view_daily_expenses(users, username):
    if username in users:
        user_data = users[username]
        print(f"Daily Expenses for user '{username}':")
        for category, expense in user_data['daily_expenses'].items():
            print(f"{category}: ${expense}")
    else:
        print("No user selected!")

# main.py
def main():
    users = load_users()
    current_user = None

    while True:
        print("\n1. Add User")
        print("2. Select User")
        print("3. Edit User Details")
        print("4. View Family Expenses")
        print("5. View Daily Expenses")
        print("6. View Income")
        print("7. View Budget")
        print("8. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            username = input("Enter username: ")
            add_user(users, username)

        elif choice == '2':
            username = input("Enter username: ")
            current_user = select_user(users, username)

        elif choice == '3':
            if current_user:
                edit_user_details(users, current_user)
            else:
                print("No user selected!")

        elif choice == '4':
            if current_user:
                view_family_expenses(users, current_user)
            else:
                print("No user selected!")

        elif choice == '5':
            if current_user:
                view_daily_expenses(users, current_user)
            else:
                print("No user selected!")

        elif choice == '6':
            if current_user:
                view_income(users, current_user)
            else:
                print("No user selected!")

        elif choice == '7':
            if current_user:
                view_budget(users, current_user)
            else:
                print("No user selected.")

        elif choice == '8':
            save_users(users)
            print("Exiting...")
            break

        else:
            print("Invalid choice! Please enter a valid option.")

if __name__ == "__main__":
    main()
