# bank_system.py

class InsufficientFundsError(Exception):
    """Custom exception for insufficient funds during withdrawal."""
    pass


class Account:
    """A class to represent a bank account."""
    
    def __init__(self, account_holder, initial_balance=0):
        """
        Initialize an account with account holder name and initial balance.

        Args:
        account_holder (str): Name of the account holder.
        initial_balance (float): Initial balance in the account. Default is 0.
        """
        self.account_holder = account_holder
        self.balance = initial_balance

    def deposit(self, amount):
        """
        Deposit an amount into the account.

        Args:
        amount (float): The amount to deposit.

        Returns:
        None
        """
        if amount <= 0:
            raise ValueError("Deposit amount must be positive.")
        self.balance += amount
        print(f"Deposited {amount}. New balance: {self.balance}")

    def withdraw(self, amount):
        """
        Withdraw an amount from the account, if sufficient balance exists.

        Args:
        amount (float): The amount to withdraw.

        Raises:
        InsufficientFundsError: If the withdrawal amount exceeds the balance.

        Returns:
        None
        """
        if amount <= 0:
            raise ValueError("Withdrawal amount must be positive.")
        if self.balance < amount:
            raise InsufficientFundsError("Insufficient funds to complete the transaction.")
        self.balance -= amount
        print(f"Withdrew {amount}. New balance: {self.balance}")

    def check_balance(self):
        """
        Check the current balance of the account.

        Returns:
        float: The current balance.
        """
        print(f"Current balance: {self.balance}")
        return self.balance


class Bank:
    """A class to represent a bank with multiple accounts."""
    
    def __init__(self):
        """Initialize the bank with an empty dictionary of accounts."""
        self.accounts = {}

    def add_account(self, account_holder, initial_balance=0):
        """
        Add a new account to the bank.

        Args:
        account_holder (str): Name of the account holder.
        initial_balance (float): Initial balance for the account.

        Returns:
        Account: The newly created account.
        """
        if account_holder in self.accounts:
            raise ValueError(f"Account already exists for {account_holder}.")
        new_account = Account(account_holder, initial_balance)
        self.accounts[account_holder] = new_account
        print(f"Account created for {account_holder}. Initial balance: {initial_balance}")
        return new_account

    def get_account(self, account_holder):
        """
        Retrieve an account by account holder name.

        Args:
        account_holder (str): Name of the account holder.

        Returns:
        Account: The account associated with the account holder.
        """
        if account_holder not in self.accounts:
            raise ValueError(f"No account found for {account_holder}.")
        return self.accounts[account_holder]

# Main function to demonstrate the usage
if __name__ == "__main__":
    bank = Bank()
    
    # Adding accounts
    account1 = bank.add_account("Alice", 1000)
    account2 = bank.add_account("Bob", 500)
    
    # Deposit and Withdraw
    account1.deposit(500)
    account1.withdraw(300)
    
    account2.deposit(200)
    try:
        account2.withdraw(800)  # This will raise InsufficientFundsError
    except InsufficientFundsError as e:
        print(e)
    
    # Check Balances
    account1.check_balance()
    account2.check_balance()
