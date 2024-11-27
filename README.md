#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>  // Include string.h for strcmp

#define ESPRESSO_BEANS 8
#define ESPRESSO_WATER 30
#define CAPPUCCINO_BEANS 8
#define CAPPUCCINO_WATER 30
#define CAPPUCCINO_MILK 70
#define MOCHA_BEANS 8
#define MOCHA_WATER 39
#define MOCHA_MILK 160
#define MOCHA_SYRUP 30

#define ADMIN_PASSWORD "8390460"  // Updated admin password
#define BEANS_THRESHOLD 10
#define WATER_THRESHOLD 50
#define MILK_THRESHOLD 100
#define SYRUP_THRESHOLD 20

// Variables for prices (can be changed)
float ESPRESSO_PRICE = 3.5;
float CAPPUCCINO_PRICE = 4.5;
float MOCHA_PRICE = 5.5;

float total_amount = 0.0;
int beans_qty = 100;  // initial quantity of beans in grams
int water_qty = 500;  // initial quantity of water in ml
int milk_qty = 300;   // initial quantity of milk in ml
int syrup_qty = 100;  // initial quantity of syrup in ml

void display_menu();
void order_coffee();
void admin_mode();
void replenish_ingredients();
void change_coffee_price();
void display_ingredients_and_sales();
void handle_payment(float price);
void check_and_alert();

int main() {
    int choice;
    while (1) {
        display_menu();
        scanf("%d", &choice);
        switch (choice) {
            case 1:
                order_coffee();
                break;
            case 2:
                admin_mode();
                break;
            case 3:
                printf("Ending application execution. Goodbye!\n");
                return 0;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
    return 0;
}

void display_menu() {
    printf("\n--- Coffee Maker Menu ---\n");
    printf("1. Order a coffee type\n");
    printf("2. Admin mode for the coffee maker operator\n");
    printf("3. End the application execution\n");
    printf("Enter your choice: ");
}

void order_coffee() {
    int choice;
    while (1) {
        printf("\n--- Coffee Types ---\n");
        if (beans_qty >= ESPRESSO_BEANS && water_qty >= ESPRESSO_WATER) {
            printf("1. Espresso (AED %.2f)\n", ESPRESSO_PRICE);
        } else {
            printf("1. Espresso (Unavailable due to temporarily insufficient ingredients)\n");
        }
        if (beans_qty >= CAPPUCCINO_BEANS && water_qty >= CAPPUCCINO_WATER && milk_qty >= CAPPUCCINO_MILK) {
            printf("2. Cappuccino (AED %.2f)\n", CAPPUCCINO_PRICE);
        } else {
            printf("2. Cappuccino (Unavailable due to temporarily insufficient ingredients)\n");
        }
        if (beans_qty >= MOCHA_BEANS && water_qty >= MOCHA_WATER && milk_qty >= MOCHA_MILK && syrup_qty >= MOCHA_SYRUP) {
            printf("3. Mocha (AED %.2f)\n", MOCHA_PRICE);
        } else {
            printf("3. Mocha (Unavailable due to temporarily insufficient ingredients)\n");
        }
        printf("0. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        if (choice == 0) return;

        float price;
        switch (choice) {
            case 1:
                if (beans_qty >= ESPRESSO_BEANS && water_qty >= ESPRESSO_WATER) {
                    price = ESPRESSO_PRICE;
                } else {
                    printf("Espresso is not available. Please select another option.\n");
                    continue;
                }
                break;
            case 2:
                if (beans_qty >= CAPPUCCINO_BEANS && water_qty >= CAPPUCCINO_WATER && milk_qty >= CAPPUCCINO_MILK) {
                    price = CAPPUCCINO_PRICE;
                } else {
                    printf("Cappuccino is not available. Please select another option.\n");
                    continue;
                }
                break;
            case 3:
                if (beans_qty >= MOCHA_BEANS && water_qty >= MOCHA_WATER && milk_qty >= MOCHA_MILK && syrup_qty >= MOCHA_SYRUP) {
                    price = MOCHA_PRICE;
                } else {
                    printf("Mocha is not available. Please select another option.\n");
                    continue;
                }
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                continue;
        }

        printf("You selected option %d. Price: AED %.2f\n", choice, price);
        printf("Confirm your selection? (1 for Yes, 0 for No): ");
        int confirm;
        scanf("%d", &confirm);
        if (!confirm) {
            continue;
        }

        handle_payment(price);

        switch (choice) {
            case 1:
                beans_qty -= ESPRESSO_BEANS;
                water_qty -= ESPRESSO_WATER;
                break;
            case 2:
                beans_qty -= CAPPUCCINO_BEANS;
                water_qty -= CAPPUCCINO_WATER;
                milk_qty -= CAPPUCCINO_MILK;
                break;
            case 3:
                beans_qty -= MOCHA_BEANS;
                water_qty -= MOCHA_WATER;
                milk_qty -= MOCHA_MILK;
                syrup_qty -= MOCHA_SYRUP;
                break;
        }

        printf("You ordered option %d. Price: AED %.2f. Thank you for your purchase!\n", choice, price);
        check_and_alert();
        return;
    }
}

void handle_payment(float price) {
    float payment = 0;
    float coin;
    while (payment < price) {
        printf("Insert coin (1 or 0.5 AED). Remaining amount: AED %.2f: ", price - payment);
        scanf("%f", &coin);
        if (coin == 1 || coin == 0.5) {
            payment += coin;
        } else {
            printf("Invalid coin. Please insert a valid coin.\n");
        }
    }
    total_amount += price;
    if (payment > price) {
        printf("Your change: AED %.2f\n", payment - price);
    }
}

void check_and_alert() {
    if (beans_qty <= BEANS_THRESHOLD) {
        printf("ALERT: Low coffee beans quantity!\n");
    }
    if (water_qty <= WATER_THRESHOLD) {
        printf("ALERT: Low water quantity!\n");
    }
    if (milk_qty <= MILK_THRESHOLD) {
        printf("ALERT: Low milk quantity!\n");
    }
    if (syrup_qty <= SYRUP_THRESHOLD) {
        printf("ALERT: Low chocolate syrup quantity!\n");
    }
}

void admin_mode() {
    char password[20];
    printf("Enter admin password: ");
    scanf("%s", password);
    if (strcmp(password, ADMIN_PASSWORD) != 0) {
        printf("Incorrect password. Exiting admin mode.\n");
        return;
    }

    int admin_choice;
    while (1) {
        printf("\n--- Admin Menu ---\n");
        printf("1. Display ingredient quantities and total sales amount\n");
        printf("2. Replenish ingredients\n");
        printf("3. Change coffee prices\n");
        printf("0. Exit admin mode\n");
        printf("Enter your choice: ");
        scanf("%d", &admin_choice);
        switch (admin_choice) {
            case 1:
                display_ingredients_and_sales();
                break;
            case 2:
                replenish_ingredients();
                break;
            case 3:
                change_coffee_price();
                break;
            case 0:
                return;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
}

void display_ingredients_and_sales() {
    printf("\n--- Ingredient Quantities ---\n");
    printf("Coffee beans: %d grams\n", beans_qty);
    printf("Water: %d milliliters\n", water_qty);
    printf("Milk: %d milliliters\n", milk_qty);
    printf("Chocolate syrup: %d milliliters\n", syrup_qty);
    printf("Total sales amount: AED %.2f\n", total_amount);
}

void replenish_ingredients() {
    beans_qty = rand() % 100 + 50;
    water_qty = rand() % 500 + 250;
    milk_qty = rand() % 300 + 100;
    syrup_qty = rand() % 100 + 50;
    printf("Ingredients have been replenished.\n");
}

void change_coffee_price() {
    int coffee_choice;
    printf("\n--- Change Coffee Price ---\n");
    printf("1. Espresso\n");
    printf("2. Cappuccino\n");
    printf("3. Mocha\n");
    printf("Enter your choice: ");
    scanf("%d", &coffee_choice);

    float new_price;
    printf("Enter new price: ");
    scanf("%f", &new_price);

    switch (coffee_choice) {
        case 1:
            ESPRESSO_PRICE = new_price;
            break;
        case 2:
            CAPPUCCINO_PRICE = new_price;
            break;
        case 3:
            MOCHA_PRICE = new_price;
            break;
        default:
            printf("Invalid choice.\n");
            break;
    }
    printf("Price updated successfully.\n");
}
