#include <stdio.h>

#define ESPRESSO_BEANS 8
#define ESPRESSO_WATER 30
#define ESPRESSO_MILK 0
#define ESPRESSO_CHOCOLATE 0
#define ESPRESSO_PRICE 3.5

#define CAPPUCCINO_BEANS 8
#define CAPPUCCINO_WATER 30
#define CAPPUCCINO_MILK 70
#define CAPPUCCINO_CHOCOLATE 0
#define CAPPUCCINO_PRICE 4.5

#define MOCHA_BEANS 8
#define MOCHA_WATER 39
#define MOCHA_MILK 160
#define MOCHA_CHOCOLATE 30
#define MOCHA_PRICE 5.5

#define ADMIN_PASSWORD "admin"
#define LOW_THRESHOLD_BEANS 10
#define LOW_THRESHOLD_WATER 100
#define LOW_THRESHOLD_MILK 50
#define LOW_THRESHOLD_CHOCOLATE 20

double total_amount = 0;
int beans = 100, water = 1000, milk = 500, chocolate = 200;

void displayCoffeeMenu() {
    printf("Available Coffee Types:\n");
    printf("%-12s: AED %-10.2f\n", "Espresso", ESPRESSO_PRICE);
    printf("%-12s: AED %-10.2f\n", "Cappuccino", CAPPUCCINO_PRICE);
    printf("%-12s: AED %-10.2f\n", "Mocha", MOCHA_PRICE);
}

int canServeEspresso() {
    return (beans >= ESPRESSO_BEANS && water >= ESPRESSO_WATER);
}

int canServeCappuccino() {
    return (beans >= CAPPUCCINO_BEANS && water >= CAPPUCCINO_WATER && milk >= CAPPUCCINO_MILK);
}

int canServeMocha() {
    return (beans >= MOCHA_BEANS && water >= MOCHA_WATER && milk >= MOCHA_MILK && chocolate >= MOCHA_CHOCOLATE);
}

void orderCoffee() {
    int choice;
    double price;
    
    while (1) {
        displayCoffeeMenu();
        printf("Select your coffee (1: Espresso, 2: Cappuccino, 3: Mocha, 0: Exit): ");
        scanf("%d", &choice);

        if (choice == 0) break;

        switch (choice) {
            case 1:
                if (canServeEspresso()) {
                    price = ESPRESSO_PRICE;
                    beans -= ESPRESSO_BEANS;
                    water -= ESPRESSO_WATER;
                } else {
                    printf("Espresso Unavailable due to temporarily insufficient ingredients.\n");
                    continue;
                }
                break;
            case 2:
                if (canServeCappuccino()) {
                    price = CAPPUCCINO_PRICE;
                    beans -= CAPPUCCINO_BEANS;
                    water -= CAPPUCCINO_WATER;
                    milk -= CAPPUCCINO_MILK;
                } else {
                    printf("Cappuccino Unavailable due to temporarily insufficient ingredients.\n");
                    continue;
                }
                break;
            case 3:
                if (canServeMocha()) {
                    price = MOCHA_PRICE;
                    beans -= MOCHA_BEANS;
                    water -= MOCHA_WATER;
                    milk -= MOCHA_MILK;
                    chocolate -= MOCHA_CHOCOLATE;
                } else {
                    printf("Mocha Unavailable due to temporarily insufficient ingredients.\n");
                    continue;
                }
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                continue;
        }

        printf("You selected: %s, Price: AED %.2f\n", choice == 1 ? "Espresso" : (choice == 2 ? "Cappuccino" : "Mocha"), price);
        
        double paid = 0;
        while (paid < price) {
            double coin;
            printf("Insert coin (1.0 or 0.5): ");
            scanf("%lf", &coin);
            if (coin == 1.0 || coin == 0.5) {
                paid += coin;
            } else {
                printf("Invalid coin! Please collect your coin and insert a valid one.\n");
            }
        }

        total_amount += price;
        printf("Total paid: AED %.2f, Change: AED %.2f\n", paid, paid - price);

        if (beans <= LOW_THRESHOLD_BEANS)
            printf("Alert: Coffee beans low!\n");
        if (water <= LOW_THRESHOLD_WATER)
            printf("Alert: Water low!\n");
        if (milk <= LOW_THRESHOLD_MILK)
            printf("Alert: Milk low!\n");
        if (chocolate <= LOW_THRESHOLD_CHOCOLATE)
            printf("Alert: Chocolate syrup low!\n");
    }
}

void adminMode() {
    char password[20];
    int choice;

    printf("Enter admin password: ");
    scanf("%s", password);
    if (strcmp(password, ADMIN_PASSWORD) != 0) {
        printf("Incorrect password!\n");
        return;
    }

    while (1) {
        printf("\nAdmin Menu:\n");
        printf("1: Display ingredients and total sales\n");
        printf("2: Replenish ingredients\n");
        printf("3: Change coffee prices\n");
        printf("0: Exit Admin Mode\n");
        printf("Select an option: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Current Ingredients:\n");
                printf("Beans: %d g\n", beans);
                printf("Water: %d ml\n", water);
                printf("Milk: %d ml\n", milk);
                printf("Chocolate: %d ml\n", chocolate);
                printf("Total Sales: AED %.2f\n", total_amount);
                break;
            case 2:
                beans = 100;
                water = 1000;
                milk = 500;
                chocolate = 200;
                printf("Ingredients replenished!\n");
                break;
            case 3:
                // Change coffee prices (for simplicity, we only allow changing one at a time)
                printf("Changing price for: (1: Espresso, 2: Cappuccino, 3: Mocha)\n");
                scanf("%d", &choice);
                if (choice == 1) {
                    printf("Enter new price for Espresso: ");
                    scanf("%lf", &ESPRESSO_PRICE);
                } else if (choice == 2) {
                    printf("Enter new price for Cappuccino: ");
                    scanf("%lf", &CAPPUCCINO_PRICE);
                } else if (choice == 3) {
                    printf("Enter new price for Mocha: ");
                    scanf("%lf", &MOCHA_PRICE);
                } else {
                    printf("Invalid option!\n");
                }
                break;
            case 0:
                return;
            default:
                printf("Invalid option!\n");
        }
    }
}

int main() {
    while (1) {
        int main_choice;
        printf("\nCoffee Maker Menu:\n");
        printf("1: Order a coffee type\n");
        printf("2: Admin mode\n");
        printf("3: End application\n");
        printf("Select an option: ");
        scanf("%d", &main_choice);

        if (main_choice == 1) {
            orderCoffee();
        } else if (main_choice == 2) {
            adminMode();
        } else if (main_choice == 3) {
            printf("Exiting application.\n");
            break;
        } else {
            printf("Invalid option! Please try again.\n");
        }
    }
    return 0;
}
