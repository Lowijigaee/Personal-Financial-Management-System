import java.util.HashMap;
import java.util.ArrayList;
import java.util.Scanner;

public class FinancialM {
    private static HashMap<String, String> users = new HashMap<>();
    private static double income = 0, expenses = 0;
    private static ArrayList<String> expenseDescriptions = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);
    private static String currentUser = "";

    public static void main(String[] args) {
        while (true) {
            System.out.println("\n===============================");
            System.out.println("      FINANCIAL MANAGEMENT      ");
            System.out.println("===============================");
            System.out.println("1. Login");
            System.out.println("2. Register");
            System.out.print("Choose an option: ");
            
            int choice = getValidIntegerInput();

            if (choice == 1) {
                login();
            } else if (choice == 2) {
                register();
            } else {
                System.out.println("Invalid choice.");
            }
        }
    }

    private static int getValidIntegerInput() {
        while (true) {
            if (scanner.hasNextInt()) {
                int choice = scanner.nextInt();
                scanner.nextLine(); // Consume the newline character
                return choice;
            } else {
                System.out.println("Invalid input. Please enter a number.");
                scanner.next(); // Consume invalid input
            }
        }
    }

    private static void login() {
        System.out.print("Enter Username: ");
        String username = scanner.nextLine();
        System.out.print("Enter 4-digit PIN: ");
        String pin = scanner.nextLine();
        
        if (users.containsKey(username) && users.get(username).equals(pin)) {
            currentUser = username;
            System.out.println("\nLogin successful.");
            manageFinances();
        } else {
            System.out.println("\nIncorrect username or PIN.");
        }
    }

    private static void register() {
        String username;
        while (true) {
            System.out.print("Enter a new Username: ");
            username = scanner.nextLine();
            if (users.containsKey(username)) {
                System.out.println("Username already exists. Please choose a different one.");
            } else {
                break;
            }
        }
        
        String pin;
        while (true) {
            System.out.print("Enter a 4-digit PIN: ");
            pin = scanner.nextLine();
            if (pin.matches("\\d{4}")) {
                break;
            } else {
                System.out.println("Invalid PIN. Please enter exactly 4 digits.");
            }
        }
        
        System.out.print("Re-enter PIN: ");
        String confirmPin = scanner.nextLine();
        
        if (pin.equals(confirmPin)) {
            users.put(username, pin);
            System.out.println("\nRegistration successful.");
        } else {
            System.out.println("\nPINs do not match.");
        }
    }

    private static void manageFinances() {
        while (true) {
            System.out.println("\n===============================");
            System.out.println("   FINANCIAL MANAGEMENT MENU   ");
            System.out.println("===============================");
            System.out.println("1. Add Income");
            System.out.println("2. Add Expenses");
            System.out.println("3. View Summary");
            System.out.println("4. View Transactions");
            System.out.println("5. Logout");
            System.out.print("Choose an option: ");
            
            int choice = getValidIntegerInput();

            switch (choice) {
                case 1:
                    addIncome();
                    break;
                case 2:
                    addExpenses();
                    break;
                case 3:
                    viewSummary();
                    promptContinue();
                    break;
                case 4:
                    viewTransactions();
                    promptContinue();
                    break;
                case 5:
                    if (confirmLogout()) {
                        System.out.println("\nLogging out...");
                        currentUser = "";
                        return;
                    }
                    break;
                default:
                    System.out.println("\nInvalid choice. Try again.");
            }
        }
    }

    private static boolean confirmLogout() {
        System.out.print("Are you sure you want to logout? (yes/no): ");
        String response = scanner.nextLine().trim().toLowerCase();
        return response.equals("yes");
    }

    private static void addIncome() {
        double amount;
        while (true) {
            System.out.print("Enter income amount: $");
    
