import java.util.Scanner;

public class ATM {

    public static void main(String[] args) {

        Scanner input = new Scanner(System.in);

        // Predefined PIN
        final int correctPIN = 1234;

        int attempts = 0;
        boolean loggedIn = false;

        // Initial balance
        double balance = 1000.00;

        // Array to store transaction history
        String[] transactions = new String[100];
        int transactionCount = 0;

        System.out.println("================================");
        System.out.println("       ATM SIMULATION SYSTEM");
        System.out.println("================================");

        // PIN Authentication
        while (attempts < 3 && !loggedIn) {

            System.out.print("Enter your PIN: ");

            if (input.hasNextInt()) {

                int enteredPIN = input.nextInt();

                if (enteredPIN == correctPIN) {

                    loggedIn = true;
                    System.out.println("Login successful. Welcome!");

                } else {

                    attempts++;
                    System.out.println("Incorrect PIN.");
                    System.out.println("Attempts left: " + (3 - attempts));
                }

            } else {

                input.next();
                attempts++;
                System.out.println("Invalid PIN.");
                System.out.println("Attempts left: " + (3 - attempts));
            }
        }

        // Stop program after 3 wrong attempts
        if (!loggedIn) {

            System.out.println("Maximum attempts reached.");
            System.out.println("Your session is locked.");

            input.close();
            return;
        }

        int choice = 0;

        // Main Menu
        while (choice != 5) {

            System.out.println("\n========== MAIN MENU ==========");
            System.out.println("1. Check Balance");
            System.out.println("2. Deposit Money");
            System.out.println("3. Withdraw Money");
            System.out.println("4. View Transaction History");
            System.out.println("5. Exit");

            System.out.print("Choose an option: ");

            if (!input.hasNextInt()) {
                input.next();
                System.out.println("Please enter a number from 1 to 5.");
                continue;
            }

            choice = input.nextInt();

            switch (choice) {

                // Check Balance
                case 1:

                    System.out.printf(
                        "Your current balance is: %.2f%n",
                        balance
                    );

                    break;

                // Deposit Money
                case 2:

                    System.out.print("Enter deposit amount: ");

                    if (!input.hasNextDouble()) {
                        input.next();
                        System.out.println("Invalid amount.");
                        break;
                    }

                    double depositAmount = input.nextDouble();

                    if (depositAmount > 0) {

                        balance += depositAmount;

                        System.out.println("Deposit successful.");

                        System.out.printf(
                            "New balance: %.2f%n",
                            balance
                        );

                        if (transactionCount < transactions.length) {

                            transactions[transactionCount] =
                                String.format(
                                    "Deposit: +%.2f | Balance: %.2f",
                                    depositAmount,
                                    balance
                                );

                            transactionCount++;
                        }

                    } else {

                        System.out.println(
                            "Amount must be greater than zero."
                        );
                    }

                    break;

                // Withdraw Money
                case 3:

                    System.out.print("Enter withdrawal amount: ");

                    if (!input.hasNextDouble()) {
                        input.next();
                        System.out.println("Invalid amount.");
                        break;
                    }

                    double withdrawalAmount = input.nextDouble();

                    if (withdrawalAmount <= 0) {

                        System.out.println(
                            "Amount must be greater than zero."
                        );

                    } else if (withdrawalAmount <= balance) {

                        balance -= withdrawalAmount;

                        System.out.println("Withdrawal successful.");

                        System.out.printf(
                            "New balance: %.2f%n",
                            balance
                        );

                        if (transactionCount < transactions.length) {

                            transactions[transactionCount] =
                                String.format(
                                    "Withdrawal: -%.2f | Balance: %.2f",
                                    withdrawalAmount,
                                    balance
                                );

                            transactionCount++;
                        }

                    } else {

                        System.out.println(
                            "Error: Insufficient balance."
                        );
                    }

                    break;

                // Transaction History
                case 4:

                    System.out.println(
                        "\n====== TRANSACTION HISTORY ======"
                    );

                    if (transactionCount == 0) {

                        System.out.println("No transactions yet.");

                    } else {

                        // Display transactions using for loop
                        for (int i = 0; i < transactionCount; i++) {

                            System.out.println(
                                (i + 1) + ". " + transactions[i]
                            );
                        }
                    }

                    break;

                // Exit
                case 5:

                    System.out.println(
                        "Thank you for using the ATM. Goodbye!"
                    );

                    break;

                default:

                    System.out.println(
                        "Invalid option. Choose from 1 to 5."
                    );
            }
        }

        input.close();
    }
}
