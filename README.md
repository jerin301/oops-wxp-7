package exp6;
import java.util.Scanner;


class InvalidATMPinException extends Exception {
    private static final long serialVersionUID = 1L;
    public InvalidATMPinException(String message) {
        super(message);
    }
}


class NoCashException extends Exception {
    private static final long serialVersionUID = 1L;
    public NoCashException(String message) {
        super(message);
    }
}


class Account {
    private String name;
    private double balance;
    private int atmPin;

    
    public Account(String name, double balance, int atmPin) {
        this.name = name;
        this.balance = balance;
        this.atmPin = atmPin;
    }

   
    public void checkPin(int enteredPin) throws InvalidATMPinException {
        if (enteredPin != atmPin) {
            throw new InvalidATMPinException("❌ Invalid ATM Pin! Please try again.");
        }
        System.out.println("✅ ATM Pin verified successfully.\n");
    }

    
    public void withdraw(double amount) throws NoCashException {
        if (amount > balance) {
            throw new NoCashException("❌ Insufficient balance! Withdrawal failed.");
        }
        balance -= amount;
        System.out.println("✅ Withdrawal successful! Remaining Balance: ₹" + balance);
    }

   
    public void deposit(double amount) {
        if (amount <= 0) {
            System.out.println("⚠️ Invalid deposit amount!");
        } else {
            balance += amount;
            System.out.println("✅ Amount deposited successfully! New Balance: ₹" + balance);
        }
    }

    
    public void checkBalance() {
        System.out.println("💰 Current Balance: ₹" + balance);
    }
}


public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

      
        Account account = new Account("John Doe", 5000.0, 1234);

        System.out.print("Enter ATM Pin: ");
        int enteredPin = sc.nextInt();

        try {
            account.checkPin(enteredPin);

            int choice;
            do {
                System.out.println("\n===== ATM MENU =====");
                System.out.println("1. Withdraw");
                System.out.println("2. Deposit");
                System.out.println("3. Check Balance");
                System.out.println("4. Exit");
                System.out.print("Enter your choice: ");
                choice = sc.nextInt();

                switch (choice) {
                    case 1:
                        System.out.print("Enter amount to withdraw: ");
                        double withdrawAmount = sc.nextDouble();
                        try {
                            account.withdraw(withdrawAmount);
                        } catch (NoCashException e) {
                            System.out.println(e.getMessage());
                        }
                        break;

                    case 2:
                        System.out.print("Enter amount to deposit: ");
                        double depositAmount = sc.nextDouble();
                        account.deposit(depositAmount);
                        break;

                    case 3:
                        account.checkBalance();
                        break;

                    case 4:
                        System.out.println("👋 Thank you for using our ATM service!");
                        break;

                    default:
                        System.out.println("⚠️ Invalid choice! Please try again.");
                }

            } while (choice != 4);

        } catch (InvalidATMPinException e) {
            System.out.println(e.getMessage());
        } finally {
            sc.close();
        }
    }
}
