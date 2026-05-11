# Group9_ACRPSystem
import java.util.*;
import java.io.*;

public class ACRPSystem {
    // 2D Array for Farmer Data: [ID, Name, Mobile, Qty, Hectares]
    static String[][] farmerData = new String[100][5]; 
    static String[][] userData = new String[15][3];

    static int recordCount = 0;
    static int userCount = 0;
    static String currentLoggedInUser = "";
    static int userRole = -1; 
    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        loadFiles();
        System.out.println("--- Welcome to Agri-Cooperative Resource Planning (ACRP) ---");

        if (login()) {
            handleMenu(userRole);
        } else {
            System.out.println("Access Denied. Incorrect Username or Password.");
        }

        saveFiles(); 
        sc.close();
    }

    // --- SECURITY & LOGIN ---
    static boolean login() {
        System.out.print("Enter Username: ");
        String inputUser = sc.nextLine();
        System.out.print("Enter Password: ");
        String inputPass = sc.nextLine();

        String encryptedInput = applyCipher(inputPass, 3);

        for (int i = 0; i < userCount; i++) {
            if (userData[i][0].equalsIgnoreCase(inputUser) && userData[i][1].equals(encryptedInput)) {
                currentLoggedInUser = userData[i][0];
                userRole = Integer.parseInt(userData[i][2]);
                System.out.println("\nLogin Successful! Welcome, " + currentLoggedInUser);
                System.out.println("Access Level: " + (userRole == 0 ? "ADMINISTRATOR" : "STAFF"));
                return true;
            }
        }
        return false;
    }

    static String applyCipher(String text, int shift) {
        StringBuilder result = new StringBuilder();
        for (char character : text.toCharArray()) {
            result.append((char) (character + shift));
        }
        return result.toString();
    }

    // --- MENUS ---
    static void handleMenu(int role) {
        boolean running = true;
        while (running) {
            System.out.println("\n================================");
            if (role == 0) {
                System.out.println("      ADMIN MAIN MENU");
                System.out.println("1. Add Farmer Record");
                System.out.println("2. View All Records & Inventory");
                System.out.println("3. Update Fertilizer Qty");
                System.out.println("4. Delete Farmer Record");
                System.out.println("5. Emergency Priority List");
                System.out.println("6. View System History");
                System.out.println("7. Logout & Exit");
            } else {
                System.out.println("      STAFF MAIN MENU");
                System.out.println("1. View All Records & Inventory");
                System.out.println("2. Emergency Priority List");
                System.out.println("3. View System History");
                System.out.println("4. Logout & Exit");
            }
            System.out.print("Select Choice: ");
            String choice = sc.nextLine();

            if (role == 0) {
                switch (choice) {
                    case "1": addFarmer(); break;
                    case "2": viewRecords(); break;
                    case "3": updateFarmer(); break;
                    case "4": deleteFarmer(); break;
                    case "5": viewEmergencyPriority(); break;
                    case "6": viewHistory(); break;
                    case "7": running = false; break;
                    default: System.out.println("Invalid choice.");
                }
            } else {
                switch (choice) {
                    case "1": viewRecords(); break;
                    case "2": viewEmergencyPriority(); break;
                    case "3": viewHistory(); break;
                    case "4": running = false; break;
                    default: System.out.println("Invalid choice.");
                }
            }
        }
    }

    // --- CORE FUNCTIONS (CRUD) ---
    static void addFarmer() {
        if (recordCount >= 100) { System.out.println("Database Full."); return; }
        
        System.out.print("Enter Farmer ID: "); 
        String id = sc.nextLine();
        
        System.out.print("Enter Mobile Number: "); 
        String mobile = sc.nextLine();

        // --- DUPLICATION CHECK ---
        for (int i = 0; i < recordCount; i++) {
            if (farmerData[i][0].equals(id)) {
                System.out.println("ERROR: Farmer ID " + id + " already exists!");
                return; 
            }
            if (farmerData[i][2].equals(mobile)) {
                System.out.println("ERROR: Mobile Number " + mobile + " is already registered!");
                return; 
            }
        }

        System.out.print("Enter Full Name: "); 
        String name = sc.nextLine();
        System.out.print("Enter Total Hectares: "); 
        String hectaresStr = sc.nextLine();
        System.out.print("Enter Fertilizer Qty (Sacks): "); 
        String qtyStr = sc.nextLine();

        double h = Double.parseDouble(hectaresStr);
        int q = Integer.parseInt(qtyStr);

        // --- FERTILIZER LIMIT LOGIC (Max 3 sacks per hectare) ---
        if (q > (h * 3)) { 
            System.out.println("!!! WARNING: " + q + " sacks exceeds the limit for " + h + " hectares (Max: " + (int)(h*3) + ") !!!");
        }

        farmerData[recordCount++] = new String[]{id, name, mobile, qtyStr, hectaresStr};
        logAction("ADDED Farmer ID: " + id + " | " + h + "ha | " + q + " sacks");
        
        // AUTO-SAVE TO FILE
        saveFiles(); 
        
        System.out.println("Record saved successfully.");

    }

    static void viewRecords() {
        if (recordCount == 0) { System.out.println("\nNo records found in database."); return; }
        
        int totalInventory = 0;
        System.out.printf("\n%-10s %-20s %-15s %-10s %-10s%n", "ID", "NAME", "MOBILE", "SACKS", "HECTARES");
        System.out.println("----------------------------------------------------------------------");
        for (int i = 0; i < recordCount; i++) {
            System.out.printf("%-10s %-20s %-15s %-10s %-10s%n",
                    farmerData[i][0], farmerData[i][1], farmerData[i][2], farmerData[i][3], farmerData[i][4]);
            
            totalInventory += Integer.parseInt(farmerData[i][3]);
        }
        System.out.println("----------------------------------------------------------------------");
        System.out.println("TOTAL INVENTORY: " + totalInventory + " sacks (" + (totalInventory * 50) + " kg)");
        
        if (totalInventory < 20) {
            System.out.println("!!! LOW COOP STOCK ALERT: Please order more from suppliers !!!");
        }
    }

    static void updateFarmer() {
        System.out.print("Enter Farmer ID to update: ");
        String id = sc.nextLine();
        for (int i = 0; i < recordCount; i++) {
            if (farmerData[i][0].equals(id)) {
                System.out.print("Enter New Fertilizer Qty (Sacks) for " + farmerData[i][1] + ": ");
                String newQty = sc.nextLine();

                double h = Double.parseDouble(farmerData[i][4]);
                int q = Integer.parseInt(newQty);

                if (q > (h * 3)) {
                    System.out.println("!!! WARNING: Exceeds limit for " + h + " hectares !!!");
                }

                logAction("UPDATED Qty for Farmer ID " + id + " to " + newQty);
                farmerData[i][3] = newQty;
                
                // AUTO-SAVE TO FILE
                saveFiles(); 
                
                System.out.println("Update successful and saved.");
                return;
            }
        }
        System.out.println("Farmer ID not found.");
    }

    static void viewEmergencyPriority() {
        System.out.println("\n--- EMERGENCY PRIORITY LIST (Below 2 sacks per hectare) ---");
        System.out.printf("%-10s %-20s %-10s %-10s%n", "ID", "NAME", "HA", "SACKS");
        System.out.println("------------------------------------------------------------");
        boolean found = false;
        for (int i = 0; i < recordCount; i++) {
            int q = Integer.parseInt(farmerData[i][3]);
            double h = Double.parseDouble(farmerData[i][4]);
            
            if (q < (h * 2)) { 
                System.out.printf("%-10s %-20s %-10s %-10s%n",
                        farmerData[i][0], farmerData[i][1], farmerData[i][4], farmerData[i][3]);
                found = true;
            }
        }
        if (!found) System.out.println("No farmers currently in emergency status.");
    }

    static void deleteFarmer() {
        System.out.print("Enter Farmer ID to delete: ");
        String id = sc.nextLine();
        for (int i = 0; i < recordCount; i++) {
            if (farmerData[i][0].equals(id)) {
                logAction("DELETED Farmer ID: " + id + " (" + farmerData[i][1] + ")");
                for (int j = i; j < recordCount - 1; j++) {
                    farmerData[j] = farmerData[j + 1];
                }
                recordCount--;
                
                // AUTO-SAVE TO FILE
                saveFiles(); 
                
                System.out.println("Record deleted and saved.");
                return;
            }
        }
        System.out.println("Farmer ID not found.");
    }

    // --- HISTORY & LOGGING ---
    static void logAction(String action) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("history.txt", true))) {
            bw.write("[" + currentLoggedInUser + "] " + action);
            bw.newLine();
        } catch (IOException e) { }
    }

    static void viewHistory() {
        System.out.println("\n--- SYSTEM AUDIT LOG ---");
        try (BufferedReader br = new BufferedReader(new FileReader("history.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("No history found.");
        }
    }

    // --- FILE HANDLING ---
    static void loadFiles() {
        try (BufferedReader br = new BufferedReader(new FileReader("users.txt"))) {
            String line;
            while ((line = br.readLine()) != null && userCount < 15) {
                String[] parts = line.split(",");
                if (parts.length == 3) userData[userCount++] = parts;
            }
        } catch (IOException e) {
            System.out.println("CRITICAL ERROR: users.txt missing!");
        }

        try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
            String line;
            while ((line = br.readLine()) != null && recordCount < 100) {
                String[] data = line.split(",");
                if (data.length == 4) {
                    farmerData[recordCount++] = new String[]{data[0], data[1], data[2], data[3], "1.0"};
                } else if (data.length == 5) {
                    farmerData[recordCount++] = data;
                }
            }
        } catch (IOException e) { }
    }

    static void saveFiles() {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("data.txt"))) {
            for (int i = 0; i < recordCount; i++) {
                bw.write(String.join(",", farmerData[i]));
                bw.newLine();
            }
        } catch (IOException e) {
            System.out.println("Error saving farmer data.");
        }
    }
}

