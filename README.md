# ODD
import java.util.HashMap;

import java.util.Scanner;



// User Class for storing user credentials

class User {

    private String username;

    private String password;



    public User(String username, String password) {

        this.username = username;

        this.password = password;

    }



    public String getUsername() {

        return username;

    }



    public boolean checkPassword(String password) {

        return this.password.equals(password);

    }

}



// CurrencyConverter Class

class CurrencyConverter {

    // Static conversion rates (for demonstration)

    private static final double USD_TO_EUR = 0.85;

    private static final double EUR_TO_USD = 1.18;

    private static final double USD_TO_GBP = 0.75;

    private static final double GBP_TO_USD = 1.33;



    public double convert(String fromCurrency, String toCurrency, double amount) {

        switch (fromCurrency.toUpperCase() + "_TO_" + toCurrency.toUpperCase()) {

            case "USD_TO_EUR":

                return amount * USD_TO_EUR;

            case "EUR_TO_USD":

                return amount * EUR_TO_USD;

            case "USD_TO_GBP":

                return amount * USD_TO_GBP;

            case "GBP_TO_USD":

                return amount * GBP_TO_USD;

            default:

                System.out.println("Conversion rate not available.");

                return -1;

        }

    }

}



// Main System with Login and Registration

public class CurrencyConverterSystem {

    private HashMap<String, User> users = new HashMap<>();

    private User loggedInUser = null;

    private CurrencyConverter converter = new CurrencyConverter();



    public static void main(String[] args) {

        CurrencyConverterSystem system = new CurrencyConverterSystem();

        system.start();

    }



    private void start() {

        Scanner scanner = new Scanner(System.in);

        boolean exit = false;



        while (!exit) {

            if (loggedInUser == null) {

                System.out.println("\n1. Register\n2. Login\n3. Exit");

                System.out.print("Enter choice: ");

                int choice = scanner.nextInt();

                scanner.nextLine();  // Consume newline



                switch (choice) {

                    case 1:

                        register(scanner);

                        break;

                    case 2:

                        login(scanner);

                        break;

                    case 3:

                        exit = true;

                        break;

                    default:

                        System.out.println("Invalid choice. Try again.");

                }

            } else {

                System.out.println("\n1. Convert Currency\n2. Logout\n3. Exit");

                System.out.print("Enter choice: ");

                int choice = scanner.nextInt();

                scanner.nextLine();  // Consume newline



                switch (choice) {

                    case 1:

                        performConversion(scanner);

                        break;

                    case 2:

                        logout();

                        break;

                    case 3:

                        exit = true;

                        break;

                    default:

                        System.out.println("Invalid choice. Try again.");

                }

            }

        }

        scanner.close();

    }



    private void register(Scanner scanner) {

        System.out.print("Enter username: ");

        String username = scanner.nextLine();

        System.out.print("Enter password: ");

        String password = scanner.nextLine();



        if (users.containsKey(username)) {

            System.out.println("Username already exists. Please choose a different one.");

        } else {

            users.put(username, new User(username, password));

            System.out.println("Registration successful! You can now log in.");

        }

    }



    private void login(Scanner scanner) {

        System.out.print("Enter username: ");

        String username = scanner.nextLine();

        System.out.print("Enter password: ");

        String password = scanner.nextLine();



        User user = users.get(username);

        if (user != null && user.checkPassword(password)) {

            loggedInUser = user;

            System.out.println("Login successful! Welcome, " + username + "!");

        } else {

            System.out.println("Invalid username or password. Please try again.");

        }

    }



    private void logout() {

        System.out.println("Logging out " + loggedInUser.getUsername());

        loggedInUser = null;

    }



    private void performConversion(Scanner scanner) {

        System.out.print("Enter amount: ");

        double amount = scanner.nextDouble();

        scanner.nextLine();  // Consume newline

        System.out.print("Enter from currency (USD, EUR, GBP): ");

        String fromCurrency = scanner.nextLine();

        System.out.print("Enter to currency (USD, EUR, GBP): ");

        String toCurrency = scanner.nextLine();



        // Ensure valid input for currency codes

        if (!(fromCurrency.equalsIgnoreCase("USD") || fromCurrency.equalsIgnoreCase("EUR") || fromCurrency.equalsIgnoreCase("GBP")) ||

            !(toCurrency.equalsIgnoreCase("USD") || toCurrency.equalsIgnoreCase("EUR") || toCurrency.equalsIgnoreCase("GBP"))) {

            System.out.println("Invalid currency code. Please use USD, EUR, or GBP.");

            return;

        }



        double result = converter.convert(fromCurrency, toCurrency, amount);

        if (result != -1) {

            System.out.printf("%.2f %s = %.2f %s%n", amount, fromCurrency.toUpperCase(), result, toCurrency.toUpperCase());

        }

    }

}


