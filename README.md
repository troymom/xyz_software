#include <iostream>
#include <string>
#include <vector>
#include <limits>

class Employee {
private:
    std::string name;
    int employeeId;
    double salary;

public:
    Employee() : employeeId(0), salary(0.0) {}

    Employee(std::string name, int employeeId, double salary) : name(name), employeeId(employeeId), salary(salary) {}

    std::string getName() const { return name; }
    int getEmployeeId() const { return employeeId; }
    double getSalary() const { return salary; }

    void setName(const std::string& name) { this->name = name; }
    void setEmployeeId(int employeeId) { this->employeeId = employeeId; }
    void setSalary(double salary) { this->salary = salary; }

    virtual void inputDetails() {
        std::cout << "Enter Employee Name: ";
        std::getline(std::cin >> std::ws, name);

        std::cout << "Enter Employee ID: ";
        while (!(std::cin >> employeeId)) {
            std::cout << "Invalid input. Please enter a numeric Employee ID: ";
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        }

        std::cout << "Enter Employee Salary: ";
        while (!(std::cin >> salary)) {
            std::cout << "Invalid input. Please enter a numeric Salary: ";
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        }
    }

    virtual void displayDetails() const {
        std::cout << "Name: " << getName() << std::endl;
        std::cout << "Employee ID: " << getEmployeeId() << std::endl;
        std::cout << "Salary: $" << getSalary() << std::endl;
    }
};

class Manager : public Employee {
private:
    std::string department;
    double bonus;

public:
    Manager() : bonus(0.0) {}

    Manager(std::string name, int employeeId, double salary, std::string department, double bonus)
        : Employee(name, employeeId, salary), department(department), bonus(bonus) {}

    std::string getDepartment() const { return department; }
    double getBonus() const { return bonus; }

    void setDepartment(const std::string& department) { this->department = department; }
    void setBonus(double bonus) { this->bonus = bonus; }


    void inputDetails() override {
        Employee::inputDetails();
        std::cout << "Enter Department: ";
        std::getline(std::cin >> std::ws, department);
        std::cout << "Enter Bonus: ";
        while (!(std::cin >> bonus)) {
            std::cout << "Invalid input. Please enter a numeric Bonus: ";
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        }
    }

    void displayDetails() const override {
        Employee::displayDetails();
        std::cout << "Department: " << getDepartment() << std::endl;
        std::cout << "Bonus: $" << getBonus() << std::endl;
    }

    void displayManagerDetails() const {
        displayDetails();
    }
};

class Engineer : public Employee {
private:
    std::string specialization;
    std::string projectAssigned;

public:
    Engineer() {}

    Engineer(std::string name, int employeeId, double salary, std::string specialization, std::string projectAssigned)
        : Employee(name, employeeId, salary), specialization(specialization), projectAssigned(projectAssigned) {}

    std::string getSpecialization() const { return specialization; }
    std::string getProjectAssigned() const { return projectAssigned; }

    void setSpecialization(const std::string& specialization) { this->specialization = specialization; }
    void setProjectAssigned(const std::string& projectAssigned) { this->projectAssigned = projectAssigned; }


    void inputDetails() override {
        Employee::inputDetails();
        std::cout << "Enter Specialization: ";
        std::getline(std::cin >> std::ws, specialization);
        std::cout << "Enter Project Assigned: ";
        std::getline(std::cin >> std::ws, projectAssigned);
    }

    void displayDetails() const override {
        Employee::displayDetails();
        std::cout << "Specialization: " << getSpecialization() << std::endl;
        std::cout << "Project Assigned: " << getProjectAssigned() << std::endl;
    }

    void displayEngineerDetails() const {
        displayDetails();
    }
};

class EmployeeManagementSystem {
private:
    std::vector<Employee*> employees;

public:
    void addEmployee(Employee* emp) {
        employees.push_back(emp);
    }

    void displayAllEmployees() const {
        if (employees.empty()) {
            std::cout << "No employees in the system." << std::endl;
            return;
        }
        std::cout << "--- Employee Details ---" << std::endl;
        for (const Employee* emp : employees) {
            emp->displayDetails();
            std::cout << "------------------------" << std::endl;
        }
    }

    void searchEmployeeById(int id) const {
        for (const Employee* emp : employees) {
            if (emp->getEmployeeId() == id) {
                std::cout << "--- Employee Found ---" << std::endl;
                emp->displayDetails();
                std::cout << "----------------------" << std::endl;
                return;
            }
        }
        std::cout << "Employee with ID " << id << " not found." << std::endl;
    }
};

int main() {
    EmployeeManagementSystem empSystem;

    while (true) {
        std::cout << "\nEmployee Management System Menu:\n";
        std::cout << "1. Add Employee\n";
        std::cout << "2. Add Manager\n";
        std::cout << "3. Add Engineer\n";
        std::cout << "4. Display All Employees\n";
        std::cout << "5. Search Employee by ID\n";
        std::cout << "6. Exit\n";
        std::cout << "Enter your choice: ";

        int choice;
        while (!(std::cin >> choice)) {
            std::cout << "Invalid input. Please enter a number from 1 to 6: ";
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        }

        if (choice == 1) {
            Employee* emp = new Employee();
            emp->inputDetails();
            empSystem.addEmployee(emp);
        } else if (choice == 2) {
            Manager* manager = new Manager();
            manager->inputDetails();
            empSystem.addEmployee(manager);
        } else if (choice == 3) {
            Engineer* engineer = new Engineer();
            engineer->inputDetails();
            empSystem.addEmployee(engineer);
        } else if (choice == 4) {
            empSystem.displayAllEmployees();
        } else if (choice == 5) {
            int searchId;
            std::cout << "Enter Employee ID to search: ";
            while (!(std::cin >> searchId)) {
                std::cout << "Invalid input. Please enter a numeric Employee ID: ";
                std::cin.clear();
                std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            }
            empSystem.searchEmployeeById(searchId);
        } else if (choice == 6) {
            std::cout << "Exiting program.\n";
            break;
        } else {
            std::cout << "Invalid choice. Please enter a number from 1 to 6.\n";
        }
    }
    return 0;
}
