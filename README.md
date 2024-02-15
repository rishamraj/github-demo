#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_CUSTOMERS 60
#define MAX_NAME_LENGTH 20

struct Customer {
    char name[20];             //creating structure for bank management
    int accountNumber;         //framework of our project
    float balance;
};

void createAccount(struct Customer bank[], int *numCustomers) {
    if (*numCustomers >= MAX_CUSTOMERS) {
        printf("The bank is at maximum capacity. Cannot create more accounts.Apologies for inconvinience. \n");
        return;
    }

    struct Customer newCustomer;
    printf("Please enter your name: ");
    getchar();
    fgets(newCustomer.name, sizeof(newCustomer.name), stdin);
    newCustomer.name[strlen(newCustomer.name) - 1] = '\0'; // Remove trailing newline character

    newCustomer.accountNumber = (*numCustomers) + 1; // Auto-generate account number
    newCustomer.balance = 0.0;

    bank[*numCustomers] = newCustomer;
    (*numCustomers)++;

    printf("Account created successfully!\n");
}

void deposit(struct Customer bank[], int numCustomers, int accountNumber, float amount) {
    int found = 0;

    for (int i = 0; i < numCustomers; i++) {
        if (bank[i].accountNumber == accountNumber) {
            found = 1;
            bank[i].balance += amount;
            printf("Deposit successful. New balance: %.2lf\n", bank[i].balance);
            break;
        }
    }

    if (!found) {
        printf("Account not found.\n");
    }
}

void withdraw(struct Customer bank[], int numCustomers, int accountNumber, double amount) {
    int found = 0;

    for (int i = 0; i < numCustomers; i++) {
        if (bank[i].accountNumber == accountNumber) {
            found = 1;
            if (bank[i].balance >= amount) {
                bank[i].balance -= amount;
                printf("Withdrawal successful. New balance: %.2lf\n", bank[i].balance);
            } else {
                printf("Insufficient balance.\n");
            }
            break;
        }
    }

    if (!found) {
        printf("Account not found.\n");
    }
}

int main() {
    struct Customer bank[MAX_CUSTOMERS];
    int numCustomers = 0;
    int choice;
    int accountNumber;
    double amount;

    printf("Bank Account Management System\n");
    printf("-------------------------------\n");

    while (1) {
        printf("\nOptions:\n");
        printf("1. Create a bank account\n");
        printf("2. Deposit funds\n");
        printf("3. Withdraw funds\n");
        printf("4. Quit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                createAccount(bank, &numCustomers);
                break;
            case 2:
                printf("Enter account number: ");
                scanf("%d", &accountNumber);
                printf("Enter the amount to deposit: ");
                scanf("%f", &amount);
                deposit(bank, numCustomers, accountNumber, amount);
                break;
            case 3:
                printf("Enter account number: ");
                scanf("%d", &accountNumber);
                printf("Enter the amount to withdraw: ");
                scanf("%f", &amount);
                withdraw(bank, numCustomers, accountNumber, amount);
                break;
            case 4:
                printf("Goodbye!\n");
                exit(0);
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}
