# Cafe-Management-System
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;


public class caf2{

    private static final String INVENTORY_FILE = "inventory.txt";
    private static final String USER_ACCOUNTS_FILE = "user_accounts.txt";
    private static final String SALES_FILE = "sales.txt";

    private static final double GST_RATE = 0.18;

    private static Map<String, Integer> inventory;
    private static Map<String, String> userAccounts;

    public static void main(String[] args) {
        // Load inventory and user accounts from files
        inventory = loadInventoryFromFile(INVENTORY_FILE);
        userAccounts = loadUserAccountsFromFile(USER_ACCOUNTS_FILE);

        // Show login screen and authenticate user
        String userId = login();

        // Show main menu to user
        while (true) {
            int choice = showMainMenuAndGetChoice();
            switch (choice) {
                case 1:
                    viewInventory();
                    break;
                case 2:
                    placeOrder(userId);
                    break;
                case 3:
                    viewSalesReport();
                    break;
                case 4:
                    // Logout and exit program
                    System.out.println("Logged out. Goodbye!");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
                    break;
            }
        }
    }

    private static String login() {
        System.out.println("Welcome to the Cafe Management System!");
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.print("Enter your mobile number: ");
            String mobileNumber = scanner.nextLine();
            if (userAccounts.containsKey(mobileNumber)) {
                // User is authenticated
                System.out.println("Login successful!");
                return mobileNumber;
            } else {
                System.out.println("Invalid mobile number. Please try again.");
            }
        }
    }

    private static int showMainMenuAndGetChoice() {
        System.out.println("Main Menu:");
        System.out.println("1. View Inventory");
        System.out.println("2. Place Order");
        System.out.println("3. View Sales Report");
        System.out.println("4. Logout");
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter your choice: ");
        return scanner.nextInt();
    }

    private static void viewInventory() {
        System.out.println("Current Inventory:");
        for (Map.Entry<String, Integer> entry : inventory.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }

    private static void placeOrder(String userId) 
    {
        System.out.println("        System.out.println(\"A1 : Coffee\" +\n" +
                "                \"A2 : Tea\" +\n" +
                "                \"A3: Sandwich\" +\n" +
                "                \"A4 : Burger\" +\n" +
                "                \"A5 : Pizza\"\nPlace Order:");
        Scanner scanner = new Scanner(System.in);
        Map<String, Integer> order = new HashMap<>();
        double subtotal = 0.0;
        while (true) 
        {
            System.out.print("Enter item code (or 'done' to finish): ");
            String itemCode = scanner.nextLine();
            if (itemCode.equals("done")) 
            {
                break;
            }
            System.out.print("Enter quantity: ");
            int quantity = scanner.nextInt();
            scanner.nextLine(); // consume newline
            if (inventory.containsKey(itemCode)) 
            {
                int availableQuantity = inventory.get(itemCode);
                if (quantity <= availableQuantity)
                {
                    // Add item to order
                    order.put(itemCode, quantity);
                    inventory.put(itemCode, availableQuantity - quantity);
                    double price = getItemPrice(itemCode) * quantity;
                    subtotal += price;
                    System.out.printf("Added %d x %s (Rs. %.2f)\n", quantity, getItemName(itemCode), price);
                } 
                else 
                {
                    System.out.println("Insufficient quantity");
                }
            }
        }
    }


private static String getItemName(String itemCode)
 {
    switch (itemCode) 
    {
        case "A1":
            return "Coffee";
        case "A2":
            return "Tea";
        case "A3":
            return "Sandwich";
        case "A4":
            return "Burger";
        case "A5":
            return "Pizza";
        default:
            return "Unknown";
    }
}
    private static double getItemPrice(String itemCode) 
    {
        switch (itemCode)
        {
            case "A1":
                return 100.0;
            case "A2":
                return 50.0;
            case "A3":
                return 120.0;
            case "A4":
                return 150.0;
            case "A5":
                return 250.0;
            default:
                return 0.0;
        }
    }
    private static HashMap<String, Integer> loadInventoryFromFile(String fileName)
    {
        HashMap<String, Integer> inventory = new HashMap<>();
        try 
        {
            File file = new File(fileName);
            Scanner scanner = new Scanner(file);
            while (scanner.hasNextLine()) 
            {
                String line = scanner.nextLine();
                String[] parts = line.split(",");
                String itemCode = parts[0];
                int quantity = Integer.parseInt(parts[1]);
                inventory.put(itemCode, quantity);
            }
            scanner.close();
        } 
        catch (Exception e) 
        {
            System.out.println(" quantity more than available.");
        } 
        return inventory;
    }
    private static HashMap<String, String> loadUserAccountsFromFile(String fileName)
    {
        HashMap<String, String> userAccounts = new HashMap<>();
        try 
        {
            File file = new File(fileName);
            Scanner scanner = new Scanner(file);
            while (scanner.hasNextLine()) {
            String line = scanner.nextLine();
            String[] parts = line.split(",");
            String mobileNumber = parts[0];
            String password = parts[1];
            userAccounts.put(mobileNumber, password);
        }
        scanner.close();
        }catch (Exception e) 
        {
            System.out.println("User account file not found.");
        }
        
        return userAccounts;
    }
    private static void viewSalesReport() {
    try {
        File file = new File("sales.txt");
        Scanner scanner = new Scanner(file);
        HashMap<String, Double> salesByItem = new HashMap<>();
        while (scanner.hasNextLine()) {
            String line = scanner.nextLine();
            String[] parts = line.split(",");
            String itemCode = parts[0];
            int quantity = Integer.parseInt(parts[1]);
            double price = Double.parseDouble(parts[2]);
            double totalSale = quantity * price;
            if (salesByItem.containsKey(itemCode)) {
                double currentTotal = salesByItem.get(itemCode);
                salesByItem.put(itemCode, currentTotal + totalSale);
            } else {
                salesByItem.put(itemCode, totalSale);
            }
        }
        scanner.close();
        if (salesByItem.isEmpty()) {
            System.out.println("No sales data found.");
        } else {
            System.out.println("Sales report:");
            for (String itemCode : salesByItem.keySet()) {
                double totalSale = salesByItem.get(itemCode);
                System.out.println(itemCode + ": " + totalSale);
            }
        }
    } catch (Exception e) {
        System.out.println("Sales file not found.");
    } 
}
      


}
