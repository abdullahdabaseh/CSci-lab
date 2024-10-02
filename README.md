# CSci-lab
#include <iostream>
#include <vector>
#include <string>

using namespace std;


struct Coffee {
    string name;
    double price;
    int water;
    int coffee;
    int milk;
};


void display_menu() {
    cout << "\n--- Coffee Ordering System ---\n";
    cout << "1. Order a coffee type\n";
    cout << "2. Admin mode for the coffee maker operator\n";
    cout << "3. End the application execution\n";
    cout << "------------------------------\n";
}


bool check_ingredients(const Coffee& coffee, int available[]) {
    return available[0] >= coffee.water && available[1] >= coffee.coffee && available[2] >= coffee.milk;
}


void order_coffee(vector<Coffee>& coffee_types, int available[]) {
    while (true) {
        cout << "\nAvailable coffee types:\n";
        for (size_t i = 0; i < coffee_types.size(); ++i) {
            if (check_ingredients(coffee_types[i], available)) {
                cout << (i + 1) << ". " << coffee_types[i].name << " - $" << coffee_types[i].price << endl;
            } else {
                cout << (i + 1) << ". " << coffee_types[i].name << " - Unavailable due to temporarily insufficient ingredients\n";
            }
        }

        cout << "Please select a coffee type (1-5) or 0 to exit: ";
        int choice;
        cin >> choice;

        if (choice == 0) {
            cout << "Exiting the order process.\n";
            return;
        }

        if (choice < 1 || choice > coffee_types.size() || !check_ingredients(coffee_types[choice - 1], available)) {
            cout << "Invalid choice or coffee unavailable. Please try again.\n";
            continue;
        }

        Coffee selected_coffee = coffee_types[choice - 1];
        cout << "You have selected " << selected_coffee.name << " for $" << selected_coffee.price << ".\n";
        
        cout << "Confirm your selection? (y/n): ";
        char confirm;
        cin >> confirm;

        if (confirm != 'y' && confirm != 'Y') {
            cout << "Order canceled. Please select again.\n";
            continue;
        }

        double total_paid = 0;
        while (total_paid < selected_coffee.price) {
            cout << "Insert coin (1 or 0.5 Dirham): ";
            double coin;
            cin >> coin;

            if (coin == 1.0 || coin == 0.5) {
                total_paid += coin;
                cout << "Total paid: $" << total_paid << endl;
            } else {
                cout << "Invalid coin inserted. Please collect the invalid coin and insert a valid one.\n";
            }
        }

        
        double change = total_paid - selected_coffee.price;
        cout << "You have bought a " << selected_coffee.name << " for $" << selected_coffee.price
             << ". Total paid: $" << total_paid << ". Change: $" << change << ".\n";

       
        available[0] -= selected_coffee.water;
        available[1] -= selected_coffee.coffee;
        available[2] -= selected_coffee.milk;

        // Check for ingredient thresholds
        if (available[0] <= 50) cout << "Alert: Water supply low!\n";
        if (available[1] <= 10) cout << "Alert: Coffee supply low!\n";
        if (available[2] <= 20) cout << "Alert: Milk supply low!\n";
    }
}

// Placeholder for admin mode
void admin_mode() {
    cout << "\n--- Admin Mode ---\n";
    // Admin functionality can be implemented here
    cout << "Admin functions are currently not implemented.\n";
}


int main() {
    // Coffee types and their ingredients
    vector<Coffee> coffee_types = {
        {"Espresso", 2.50, 50, 18, 0},
        {"Latte", 3.00, 50, 18, 150},
        {"Cappuccino", 3.50, 50, 18, 100},
        {"Americano", 2.00, 100, 18, 0},
        {"Mocha", 3.75, 50, 18, 100}
    };

    // Available ingredients: water, coffee, milk
    int available_ingredients[3] = {500, 100, 300}; // Water, Coffee, Milk

    while (true) {
        display_menu();
        int user_choice;

        cout << "Please choose an option (1-3): ";
        cin >> user_choice;

        switch (user_choice) {
            case 1:
                order_coffee(coffee_types, available_ingredients);
                break;
            case 2:
                admin_mode();
                break;
            case 3:
                cout << "Ending the application execution. Goodbye!\n";
                return 0;
            default:
                cout << "Invalid option. Please choose again.\n";
                break;
        }
    }

    return 0;
}
