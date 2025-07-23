
#include <iostream>
#include <fstream>
#include <string>
#include <ctime>
#include <vector>
#include <iomanip>
using namespace std;

class Account {
public:
    string accNumber;
    string name;
    double balance;

    void createAccount() {
        cout << "Enter Account Number: "; cin >> accNumber;
        cout << "Enter Name: "; cin.ignore(); getline(cin, name);
        cout << "Enter Initial Balance: "; cin >> balance;
        saveToFile();
        cout << "Account created successfully.\n";
    }

    void deposit(double amount) {
        balance += amount;
        updateFile();
        logTransaction("Deposit", amount);
    }

    void withdraw(double amount) {
        if (amount > balance) {
            cout << "Insufficient balance.\n";
            return;
        }
        balance -= amount;
        updateFile();
        logTransaction("Withdraw", amount);
    }

    void displayAccount() const {
        cout << "Account Number: " << accNumber << "\nName: " << name << "\nBalance: Rs." << fixed << setprecision(2) << balance << "\n";
    }

    void viewTransactions() {
        ifstream file("transactions.dat");
        string line;
        cout << "\nTransaction History for Account: " << accNumber << "\n";
        while (getline(file, line)) {
            if (line.find(accNumber) != string::npos)
                cout << line << endl;
        }
        file.close();
    }

    void saveToFile() {
        ofstream file("accounts.dat", ios::app);
        file << accNumber << '|' << name << '|' << balance << '\n';
        file.close();
    }

    void updateFile();
    void logTransaction(const string& type, double amount);
};

void Account::logTransaction(const string& type, double amount) {
    ofstream tfile("transactions.dat", ios::app);
    time_t now = time(0);
    string dt = ctime(&now);
    dt.pop_back(); // remove trailing newline
    tfile << dt << " | " << accNumber << " | " << type << " | Rs." << amount << '\n';
    tfile.close();
}

void Account::updateFile() {
    ifstream infile("accounts.dat");
    ofstream temp("temp.dat");
    string line;
    while (getline(infile, line)) {
        size_t pos1 = line.find('|');
        size_t pos2 = line.rfind('|');
        string ac = line.substr(0, pos1);
        string nm = line.substr(pos1 + 1, pos2 - pos1 - 1);
        double bal = stod(line.substr(pos2 + 1));

        if (ac == accNumber)
            temp << accNumber << '|' << name << '|' << balance << '\n';
        else
            temp << ac << '|' << nm << '|' << bal << '\n';
    }
    infile.close();
    temp.close();
    remove("accounts.dat");
    rename("temp.dat", "accounts.dat");
}

Account findAccount(const string& number) {
    ifstream file("accounts.dat");
    Account acc;
    string line;
    while (getline(file, line)) {
        size_t pos1 = line.find('|');
        size_t pos2 = line.rfind('|');
        string accNo = line.substr(0, pos1);
        if (accNo == number) {
            acc.accNumber = accNo;
            acc.name = line.substr(pos1 + 1, pos2 - pos1 - 1);
            acc.balance = stod(line.substr(pos2 + 1));
            break;
        }
    }
    file.close();
    return acc;
}

void adminViewAllAccounts() {
    ifstream file("accounts.dat");
    string line;
    cout << "\nAll Accounts:\n";
    while (getline(file, line)) {
        size_t pos1 = line.find('|');
        size_t pos2 = line.rfind('|');
        cout << "Acc#: " << line.substr(0, pos1);
        cout << ", Name: " << line.substr(pos1 + 1, pos2 - pos1 - 1);
        cout << ", Balance: Rs." << line.substr(pos2 + 1) << endl;
    }
    file.close();
}

bool adminLogin() {
    string username, password;
    cout << "Enter Admin Username: "; cin >> username;
    cout << "Enter Admin Password: "; cin >> password;
    return (username == "admin" && password == "admin123");
}

int main() {
    int choice;
    string accNo;
    Account acc;

    if (!adminLogin()) {
        cout << "\nInvalid credentials. Access denied.\n";
        return 1;
    }

    do {
        cout << "\n==== Smart ATM Menu ====\n";
        cout << "1. Create Account\n2. Deposit\n3. Withdraw\n4. View Account\n5. View Transactions\n6. Admin: View All Accounts\n0. Exit\n";
        cout << "Enter your choice: "; cin >> choice;

        switch (choice) {
            case 1:
                acc.createAccount(); break;
            case 2:
                cout << "Enter Account Number: "; cin >> accNo;
                acc = findAccount(accNo);
                double dep; cout << "Enter amount to deposit: "; cin >> dep;
                acc.deposit(dep); break;
            case 3:
                cout << "Enter Account Number: "; cin >> accNo;
                acc = findAccount(accNo);
                double wd; cout << "Enter amount to withdraw: "; cin >> wd;
                acc.withdraw(wd); break;
            case 4:
                cout << "Enter Account Number: "; cin >> accNo;
                acc = findAccount(accNo);
                acc.displayAccount(); break;
            case 5:
                cout << "Enter Account Number: "; cin >> accNo;
                acc = findAccount(accNo);
                acc.viewTransactions(); break;
            case 6:
                adminViewAllAccounts(); break;
            case 0:
                cout << "Exiting...\n"; break;
            default:
                cout << "Invalid option. Try again.\n";
        }
    } while (choice != 0);

    return 0;
}
