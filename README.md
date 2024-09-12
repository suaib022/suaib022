- 👋 Hi, I’m Safi
- 👀 I’m interested in App & Web Development
- 🌱 I’m currently learning Redux


#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Base class for common vehicle details
class Vehicle {
protected:
    string brand;
    string model;
    int manufacturingYear;
    string registrationPlate;

public:
    Vehicle(string b, string m, int y, string r)
        : brand(b), model(m), manufacturingYear(y), registrationPlate(r) {}

    virtual void display() const {
        cout << "Brand: " << brand << endl;
        cout << "Model: " << model << endl;
        cout << "Manufacturing Year: " << manufacturingYear << endl;
        cout << "Registration Plate: " << registrationPlate << endl;
    }

    virtual ~Vehicle() {}
};

// Derived class for specific vehicle types
class Car : public Vehicle {
public:
    Car(string b, string m, int y, string r) : Vehicle(b, m, y, r) {}

    void display() const override {
        cout << "Vehicle Type: Car" << endl;
        Vehicle::display();
    }
};

// Other vehicle types can be added similarly, e.g., Truck, Motorcycle, etc.
// Example:
class Truck : public Vehicle {
public:
    Truck(string b, string m, int y, string r) : Vehicle(b, m, y, r) {}

    void display() const override {
        cout << "Vehicle Type: Truck" << endl;
        Vehicle::display();
    }
};

// Class to hold booking details
class Booking {
private:
    string customerName;
    string serviceName;
    string timeSlot;
    Vehicle* vehicle; // Pointer to Vehicle to handle different vehicle types

public:
    Booking(string name, string service, string time, Vehicle* v)
        : customerName(name), serviceName(service), timeSlot(time), vehicle(v) {}

    void display() const {
        cout << "Customer Name: " << customerName << endl;
        cout << "Service Name: " << serviceName << endl;
        cout << "Time Slot: " << timeSlot << endl;
        if (vehicle) {
            vehicle->display();
        }
    }

    ~Booking() {
        delete vehicle; // Clean up dynamically allocated vehicle
    }
};

// BookingSystem class to manage bookings
class BookingSystem {
private:
    vector<Booking*> bookings;

public:
    void addBooking() {
        string customerName, serviceName, timeSlot, vehicleType, brand, model, registrationPlate;
        int manufacturingYear;

        cout << "Enter customer name: ";
        getline(cin, customerName);

        cout << "Available services: Wash, Polish, Detail\n";
        cout << "Enter service name: ";
        getline(cin, serviceName);

        cout << "Available time slots: Morning, Afternoon, Evening\n";
        cout << "Enter time slot: ";
        getline(cin, timeSlot);

        cout << "Available vehicle types: Car, Truck, Van, Motorcycle, Bus, Electric Vehicle, Bicycle, Tractor\n";
        cout << "Enter vehicle type: ";
        getline(cin, vehicleType);

        cout << "Enter vehicle brand: ";
        getline(cin, brand);

        cout << "Enter vehicle model: ";
        getline(cin, model);

        cout << "Enter manufacturing year: ";
        cin >> manufacturingYear;
        cin.ignore(); // Ignore newline character left by cin

        cout << "Enter registration plate: ";
        getline(cin, registrationPlate);

        Vehicle* vehicle = nullptr;
        if (vehicleType == "Car") {
            vehicle = new Car(brand, model, manufacturingYear, registrationPlate);
        } else if (vehicleType == "Truck") {
            vehicle = new Truck(brand, model, manufacturingYear, registrationPlate);
        }
        // Additional vehicle types should be handled similarly

        if (vehicle) {
            bookings.push_back(new Booking(customerName, serviceName, timeSlot, vehicle));
            cout << "Booking successfully added!\n";
        } else {
            cout << "Invalid vehicle type.\n";
        }
    }

    void viewBookings() const {
        if (bookings.empty()) {
            cout << "No bookings available.\n";
            return;
        }

        cout << "Current bookings:\n";
        for (const auto& booking : bookings) {
            booking->display();
            cout << "----------------------------\n";
        }
    }

    ~BookingSystem() {
        for (auto& booking : bookings) {
            delete booking;
        }
    }
};

int main() {
    BookingSystem system;
    int choice;

    while (true) {
        cout << "1. Add Booking\n";
        cout << "2. View Bookings\n";
        cout << "3. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore(); // Ignore newline character left by cin

        switch (choice) {
            case 1:
                system.addBooking();
                break;
            case 2:
                system.viewBookings();
                break;
            case 3:
                cout << "Exiting...\n";
                return 0;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    }

    return 0;
}
