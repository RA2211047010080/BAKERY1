#include <iostream>
#include <vector>
#include <string>
#include <iomanip> // For formatting output

using namespace std;

class Product {
public:
    string name;
    double price;
    int quantity;

    Product(string n, double p, int q) : name(n), price(p), quantity(q) {}
};

class OrderItem {
public:
    Product product;
    int quantity;

    OrderItem(Product p, int q) : product(p), quantity(q) {}
};

class Bakery {
public:
    vector<Product> products;
    vector<OrderItem> cart;
    double dailySales;

    Bakery() : dailySales(0.0) {
        // Add some sample products to the inventory
        products.push_back(Product("Bread", 2.5, 100));
        products.push_back(Product("Cake", 15.0, 30));
        products.push_back(Product("Cupcake", 1.5, 50));
    }

    void displayProducts() {
        cout << "Available Products:\n";
        for (size_t i = 0; i < products.size(); i++) {
            cout << i + 1 << ". " << products[i].name << " | Price: $" << products[i].price << " | Quantity: " << products[i].quantity << "\n";
        }
    }

    void addToCart(int productIndex, int quantity) {
        if (productIndex >= 1 && productIndex <= products.size()) {
            cart.push_back(OrderItem(products[productIndex - 1], quantity));
        } else {
            cout << "Invalid product index. Please choose a valid product.\n";
        }
    }

    void viewCart() {
        if (cart.empty()) {
            cout << "Your Cart is Empty.\n";
        } else {
            cout << "\nYour Cart:\n";
            cout << setw(2) << "ID" << setw(15) << "Product" << setw(10) << "Quantity" << setw(10) << "Price" << setw(10) << "Total" << "\n";
            cout << "---------------------------------------------\n";
            int id = 1;
            double totalAmount = 0.0;
            for (const OrderItem& item : cart) {
                double itemTotal = item.quantity * item.product.price;
                totalAmount += itemTotal;
                cout << setw(2) << id << setw(15) << item.product.name << setw(10) << item.quantity << setw(10) << "$" << item.product.price << setw(10) << "$" << itemTotal << "\n";
                id++;
            }
            cout << "---------------------------------------------\n";
            cout << "Total Amount: $" << totalAmount << endl;
        }
    }

    void calculateTotal() {
        double totalAmount = 0;
        for (const OrderItem& item : cart) {
            totalAmount += item.quantity * item.product.price;
        }
        dailySales += totalAmount;
    }

    void checkout() {
        if (cart.empty()) {
            cout << "Your Cart is Empty. Please add items to the cart before checking out.\n";
        } else {
            viewCart();
            calculateTotal();
            cout << "Thank you for your order!\n";
        }
    }

    void displayDailySales() {
        cout << "\nSales Details for the Day:\n";
        cout << "Total Sales for the Day: $" << fixed << setprecision(2) << dailySales << endl;
    }

    void collectFeedback() {
        cout << "\nCustomer Feedback:\n";
        string feedback;
        cin.ignore(); // Clear the input buffer
        cout << "Please enter your feedback and comments: ";
        getline(cin, feedback);
        cout << "Thank you for your feedback!\n";
    }
};

class Admin {
public:
    string username;
    string password;

    Admin(string user, string pass) : username(user), password(pass) {}
};

int main() {
    cout << "Welcome to Sweet Bakery!" << endl;
    Bakery bakery;
    Admin admin("admin", "admin123"); // Define admin username and password
    bool isAdminLoggedIn = false; // Flag to track admin login
    int choice;

    while (true) {
        if (!isAdminLoggedIn) {
            string enteredUsername, enteredPassword;
            cout << "Admin Login:\n";
            cout << "Username: ";
            cin >> enteredUsername;
            cout << "Password: ";
            cin >> enteredPassword;

            if (enteredUsername == admin.username && enteredPassword == admin.password) {
                isAdminLoggedIn = true;
                cout << "Admin login successful.\n";
            } else {
                cout << "Invalid username or password. Please try again.\n";
            }
        } else {
            cout << "\nCustomer Panel:\n";
            cout << "1. Display Products\n2. Add to Cart\n3. View Cart\n4. Checkout\n5. View Daily Sales\n6. Provide Feedback\n7. Exit\n";
            cout << "Enter your choice: ";
            cin >> choice;

            switch (choice) {
                case 1:
                    bakery.displayProducts();
                    break;
                case 2:
                    int productIndex, quantity;
                    cout << "Enter the product index and quantity: ";
                    cin >> productIndex >> quantity;
                    bakery.addToCart(productIndex, quantity);
                    break;
                case 3:
                    bakery.viewCart();
                    break;
                case 4:
                    bakery.checkout();
                    break;
                case 5:
                    bakery.displayDailySales();
                    break;
                case 6:
                    bakery.collectFeedback();
                    break;
                case 7:
                    cout << "Exiting Sweet Bakery. Have a great day!\n";
                    return 0;
                default:
                    cout << "Invalid choice. Please select a valid option.\n";
                    break;
            }
        }
    }

    return 0;
}

