- 👋 Hi, I’m @tombarak
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
tombarak/tombarak is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import jdk.internal.icu.text.UnicodeSet;
import jdk.javadoc.doclet.StandardDoclet;
import java.util.ArrayList;
import java.util.List;
import java.io.*;


class MyMainClass {


    // Interface IAccount
    interface IAccount {
        void Deposit(double amount);

        double Withdraw(double amount);

        double GetCurrentBalance();

        int GetAccountNumber();
    }

    //class PremiumAccount
    static class PremiumAccount implements IAccount {
        int accountNumber;
        double balance;

        public PremiumAccount(int account_Number) {
            balance = 0;
            accountNumber = account_Number;
        }

        public void Deposit(double amount) {
            balance += amount;
        }

        public double Withdraw(double amount) {
            balance = balance - amount;
            return amount;
        }

        public double GetCurrentBalance() {
            return balance;
        }

        public int GetAccountNumber() {
            return accountNumber;
        }
    }

    //class StandardAccount
    static class StandardAccount implements IAccount {
        int accountNumber;
        double balance;
        double creditLimit;

        public StandardAccount(int account_Number, double credit_Limit) {
            balance = 0;
            accountNumber = account_Number;
            if (credit_Limit < 0)
                creditLimit = credit_Limit;
            else
                creditLimit = 0;
        }

        public void Deposit(double amount) {
            balance += amount;
        }

        public double Withdraw(double amount) {
            double temp = balance + (-1 * creditLimit);
            if (amount > (-1 * (creditLimit) + balance)) {
                balance = creditLimit;
                return temp;
            } else {
                balance = balance - amount;
                return amount;
            }
        }

        public double GetCurrentBalance() {
            return balance;
        }


        public int GetAccountNumber() {
            return accountNumber;
        }
    }

    //class BasicAccount
    static class BasicAccount implements IAccount {
        int accountNumber;
        double balance;
        double creditLimit = 0;
        double withdrawalLimit;

        public BasicAccount(int account_Number, double withdrawal_Limit) {
            balance = 0;
            accountNumber = account_Number;
            withdrawalLimit = withdrawal_Limit;
        }

        public void Deposit(double amount) {
            balance += amount;
        }

        public double Withdraw(double amount) {
            double temp = balance;
            if (amount > withdrawalLimit) {
                if (withdrawalLimit <= balance) {
                    balance -= withdrawalLimit;
                    return withdrawalLimit;
                } else
                    balance = 0;
            } else {
                if (amount <= balance) {
                    balance -= amount;
                    return amount;
                } else
                    balance = 0;
            }
            return temp;
        }

        public double GetCurrentBalance() {
            return balance;
        }



        public int GetAccountNumber() {
            return accountNumber;
        }
    }


    //Interface Bank
    interface IBank {
        void OpenAccount(IAccount account);

        void CloseAccount(int accountNumber);

        List<IAccount> GetAllAccounts();

        List<IAccount> GetAllAccountsInDebt();

        List<IAccount> GetAllAccountsWithBalance(double balanceAbove);
    }

    //class Bank
    static class Bank implements IBank {
        List<IAccount> accountsList = new ArrayList<IAccount>();

        public Bank() {
        }

        public void OpenAccount(IAccount account) {
            accountsList.add(account);
        }




        public void CloseAccount(int account_Number) {
            for (int i = 0; i < accountsList.size(); i++) {
                if (accountsList.get(i).GetAccountNumber() == account_Number) {
                    if (accountsList.get(i).GetCurrentBalance() >= 0)
                        accountsList.remove(i);
                    else
                        System.out.println("The account cannot be closed due to debt");
                }
            }
        }

        public List<IAccount> GetAllAccounts() {
            return accountsList;
        }



        public List<IAccount> GetAllAccountsInDebt() {
            List<IAccount> newList = new ArrayList<>();
            int j = 0;
            for (int i = 0; i < accountsList.size(); i++) {
                if ((accountsList.get(i).GetCurrentBalance()) < 0) {
                    IAccount temp = accountsList.get(i);
                    newList.add(j, temp);
                    j++;
                }
            }
            return newList;
        }

        public List<IAccount> GetAllAccountsWithBalance(double balanceAbove) {
            List<IAccount> newList = new ArrayList<>();
            int j = 0;
            for (int i = 0; i < accountsList.size(); i++) {
                if (balanceAbove <= accountsList.get(i).GetCurrentBalance()) {
                    IAccount temp = accountsList.get(i);
                    newList.add(j, temp);
                    j++;
                }
            }
            return newList;
        }
    }

