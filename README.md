# EntriProjects
# Banking application:

import pickle
import random

def generate_account_number():
    # Generate a random 10-digit account number
    return ''.join(str(random.randint(0, 9)) for _ in range(10))

def acc_creation():
    # Opening or creating the dictionary
    try:
        with open('user_data.pkl', 'rb') as file:
            d1 = pickle.load(file)
    except FileNotFoundError:
        d1 = {}

    # User input for account creation
    username = input("Create a Username: ")
    while True:
        password = input("Create a Password: ")
        conf_password = input("Confirm Password: ")

        if password != conf_password:
            print("\nConfirm password is wrong.")
        else:
            balance = int(input("Minimum Balance: "))
            # Generate an account number
            account_number = generate_account_number()
            print(f"Your account number: {account_number}")
            # Update the dictionary with the new username, password, and account number
            d1[username] = {'password': password, 'account_number': account_number, "balance": balance}

            # Save the updated dictionary to the pickle file
            with open('user_data.pkl', 'ab') as file:  # Use 'ab' mode to append to the file
                pickle.dump(d1, file)

            print("\nAccount Successfully created")
            print(d1)
            break

def transfer(name, password, account_number, balance):
    import pickle

    with open('user_data.pkl', 'rb') as file:
        d1 = pickle.load(file)
    print(f"\ncurrent balance: {balance}")
    to_send = input("To: ")
    sent = int(input("amount: "))

    mpin = input("Mpin / Password: ")
    transfer_success = False
    while transfer_success == False:
        if mpin == password:
            balance -= sent
            print(f"current balance: {balance}")
            d1[name]['balance'] = balance
            with open('user_data.pkl', 'wb') as file:  # Use 'wb' mode to write to the file
                pickle.dump(d1, file)
            print("Transfer Successful!")
            transfer_success = True
        else:
            print("Transfer Unsuccessful")

with open('user_data.pkl', 'rb') as file:
    d1 = pickle.load(file)

print("data", d1)

while True:
    permission = input("\nWELCOME TO INT- BANKING,"
                       "\n   WHERE THE BANKING CONNECTS INTERNATIONALLY"
                       "\n1: Login\n2: Account Creation\n3: Exit\nChoose an option: ")

    if permission == "1":
        name = input("Username: ")
        login_password = input("Password: ")

        login_successful = False
        for stored_name, user_data in d1.items():
            if stored_name == name and user_data['password'] == login_password:
                print(f"\nLogin Successful\n{name.upper()} of account number {user_data['account_number']}")
                login_successful = True
                transfer(name, login_password, user_data['account_number'], user_data['balance'])
                break  # Exit the loop once login is successful

        if not login_successful:
            print("Incorrect Username & Password")

    elif permission == "2":
        acc_creation()

    elif permission == "3":
        break
