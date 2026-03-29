import mysql.connector as con
# Connect to MySQL
try:
    dbo = con.connect(
        host='localhost',
        user='root',
        password='123456root',
        database='bank_management_system'
    )
    cursor = dbo.cursor()
except con.errors.ProgrammingError as e:
    print("Error connecting to database:", e)
    exit()

# Main Menu
def main_menu():
    while True:
        print("\n==== Bank Management System ====")
        print("1. Create Account")
        print("2. View Account")
        print("3. Update Phone/Email")
        print("4. Delete Account")
        print("5. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            create_account()
        elif choice == '2':
            view_account()
        elif choice == '3':
            update_account()
        elif choice == '4':
            delete_account()
        elif choice == '5':
            print("Exiting program.")
            break
        else:
            print("Invalid choice. Please try again.")

# Create Account
def create_account():
    acc_no = int(input("Enter account number: "))
    name = input("Enter account holder's name: ")
    phone = input("Enter phone number: ")
    email = input("Enter email: ")
    address = input("Enter address: ")
    balance = float(input("Enter initial balance: "))
    loan = input("Loan taken (Yes/No): ")

    query = """
        INSERT INTO acc_holder 
        (acc_no, acc_holder_name, phone_number, email, address, initial_balance, loan_taken)
        VALUES (%s, %s, %s, %s, %s, %s, %s)
    """
    values = (acc_no, name, phone, email, address, balance, loan)
    try:
        cursor.execute(query, values)
        dbo.commit()
        print("Account created successfully!")
    except Exception as e:
        print("Error:", e)

# View Account
def view_account():
    acc_no = int(input("Enter account number to view: "))
    query = "SELECT * FROM acc_holder WHERE acc_no = %s"
    cursor.execute(query, (acc_no,))
    result = cursor.fetchone()
    if result:
        print("\nAccount Details:")
        print("Account No:", result[0])
        print("Name:", result[1])
        print("Phone:", result[2])
        print("Email:", result[3])
        print("Address:", result[4])
        print("Balance:", result[5])
        print("Loan Taken:", result[6])
    else:
        print("Account not found.")

# Update Phone/Email
def update_account():
    acc_no = int(input("Enter account number to update: "))
    print("1. Update Phone Number")
    print("2. Update Email")
    choice = input("Enter your choice: ")

    if choice == '1':
        new_phone = input("Enter new phone number: ")
        query = "UPDATE acc_holder SET phone_number = %s WHERE acc_no = %s"
        cursor.execute(query, (new_phone, acc_no))
    elif choice == '2':
        new_email = input("Enter new email: ")
        query = "UPDATE acc_holder SET email = %s WHERE acc_no = %s"
        cursor.execute(query, (new_email, acc_no))
    else:
        print("Invalid choice.")
        return

    dbo.commit()
    print("Account updated successfully.")

# Delete Account
def delete_account():
    acc_no = int(input("Enter account number to delete: "))
    query = "DELETE FROM acc_holder WHERE acc_no = %s"
    cursor.execute(query, (acc_no,))
    dbo.commit()
    print("Account deleted successfully.")

# Run the main program
main_menu()
 
