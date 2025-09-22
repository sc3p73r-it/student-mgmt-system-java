# 📂 Student Record Management System
This system provides a simple command-line interface for administrators to manage student records, including adding, updating, and viewing student information.

### 📜 Java Source Code
Here is the complete Java code for the system. It is structured into two classes: Student to represent the data for a single student, and StudentManagementSystem which contains the main logic and user interface.

```
import java.util.ArrayList;
import java.util.InputMismatchException;
import java.util.Scanner;

/**
 * Represents a single student with their details.
 */
class Student {
    // Using private fields to encapsulate student data
    private String name;
    private String id;
    private int age;
    private String grade;

    // Constructor to create a new Student object
    public Student(String name, String id, int age, String grade) {
        this.name = name;
        this.id = id;
        this.age = age;
        this.grade = grade;
    }

    // Getter and Setter methods for each attribute
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getId() {
        return id;
    }

    // ID is typically not changed, so a setter is omitted for robustness.
    // public void setId(String id) { this.id = id; }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getGrade() {
        return grade;
    }

    public void setGrade(String grade) {
        this.grade = grade;
    }

    /**
     * Overriding the toString() method for easy display of student details.
     * @return A formatted string with all student information.
     */
    @Override
    public String toString() {
        return "----------------------------------\n" +
               "Student ID:   " + id + "\n" +
               "Name:         " + name + "\n" +
               "Age:          " + age + "\n" +
               "Grade:        " + grade + "\n" +
               "----------------------------------";
    }
}

/**
 * Main class to manage student records via a console interface.
 * It uses static variables and methods to handle the student list and operations.
 */
public class StudentMgmtSystem {

    // Static list to store all student objects. An ArrayList is used for dynamic resizing.
    private static ArrayList<Student> studentList = new ArrayList<>();
    // Scanner object is declared static to be shared across all methods in this class.
    private static Scanner scanner = new Scanner(System.in);

    /**
     * The main method that runs the administrator interface menu.
     */
    public static void main(String[] args) {
        int choice;
        do {
            displayMenu();
            try {
                System.out.print("Enter your choice: ");
                choice = scanner.nextInt();
                scanner.nextLine(); // Consume the newline character left by nextInt()

                switch (choice) {
                    case 1:
                        addStudent();
                        break;
                    case 2:
                        updateStudent();
                        break;
                    case 3:
                        viewStudentDetails();
                        break;
                    case 4:
                        System.out.println("Exiting the system. Goodbye! 👋");
                        break;
                    default:
                        System.out.println("❌ Invalid choice. Please enter a number between 1 and 4.");
                }
            } catch (InputMismatchException e) {
                System.out.println("❌ Invalid input. Please enter a number.");
                scanner.nextLine(); // Clear the invalid input from the scanner
                choice = 0; // Reset choice to loop again
            }
            System.out.println(); // Add a blank line for better readability
        } while (choice != 4);
    }

    /**
     * Displays the main menu options to the administrator.
     */
    public static void displayMenu() {
        System.out.println("===== Student Record Management System =====");
        System.out.println("1. Add New Student");
        System.out.println("2. Update Student Information");
        System.out.println("3. View Student Details");
        System.out.println("4. Exit");
        System.out.println("==========================================");
    }

    /**
     * Logic to add a new student to the system.
     * Prompts for student details and validates input.
     */
    public static void addStudent() {
        System.out.println("--- Add New Student ---");
        System.out.print("Enter Student ID: ");
        String id = scanner.nextLine();

        // Check if student ID already exists
        if (findStudentById(id) != null) {
            System.out.println("❌ Error: A student with this ID already exists.");
            return;
        }

        System.out.print("Enter Student Name: ");
        String name = scanner.nextLine();

        int age = 0;
        boolean validAge = false;
        while (!validAge) {
            try {
                System.out.print("Enter Student Age: ");
                age = scanner.nextInt();
                scanner.nextLine(); // Consume newline
                if (age > 0) {
                    validAge = true;
                } else {
                    System.out.println("❌ Age must be a positive number.");
                }
            } catch (InputMismatchException e) {
                System.out.println("❌ Invalid input for age. Please enter a number.");
                scanner.nextLine(); // Clear buffer
            }
        }

        System.out.print("Enter Student Grade (e.g., '10th', 'A+', 'First Year'): ");
        String grade = scanner.nextLine();

        Student newStudent = new Student(name, id, age, grade);
        studentList.add(newStudent);
        System.out.println("✅ Student added successfully!");
    }

    /**
     * Logic to update an existing student's information.
     */
    public static void updateStudent() {
        System.out.println("--- Update Student Information ---");
        System.out.print("Enter the ID of the student to update: ");
        String id = scanner.nextLine();

        Student studentToUpdate = findStudentById(id);

        if (studentToUpdate == null) {
            System.out.println("❌ Error: Student with ID '" + id + "' not found.");
            return;
        }

        System.out.println("Current details:");
        System.out.println(studentToUpdate);

        System.out.print("Enter new Name (or press Enter to keep current): ");
        String newName = scanner.nextLine();
        if (!newName.isEmpty()) {
            studentToUpdate.setName(newName);
        }

        System.out.print("Enter new Age (or press Enter to keep current): ");
        String ageInput = scanner.nextLine();
        if (!ageInput.isEmpty()) {
            try {
                int newAge = Integer.parseInt(ageInput);
                if (newAge > 0) {
                    studentToUpdate.setAge(newAge);
                } else {
                    System.out.println("⚠️ Invalid age. Keeping the current value.");
                }
            } catch (NumberFormatException e) {
                System.out.println("⚠️ Invalid number format for age. Keeping the current value.");
            }
        }

        System.out.print("Enter new Grade (or press Enter to keep current): ");
        String newGrade = scanner.nextLine();
        if (!newGrade.isEmpty()) {
            studentToUpdate.setGrade(newGrade);
        }

        System.out.println("✅ Student information updated successfully!");
    }

    /**
     * Logic to view the details of a specific student.
     */
    public static void viewStudentDetails() {
        System.out.println("--- View Student Details ---");
        System.out.print("Enter the ID of the student to view: ");
        String id = scanner.nextLine();

        Student student = findStudentById(id);

        if (student == null) {
            System.out.println("❌ Error: Student with ID '" + id + "' not found.");
        } else {
            System.out.println("Showing details for student " + id + ":");
            System.out.println(student);
        }
    }

    /**
     * Helper method to find a student in the studentList by their ID.
     * @param id The ID of the student to search for.
     * @return The Student object if found, otherwise null.
     */
    private static Student findStudentById(String id) {
        for (Student student : studentList) {
            if (student.getId().equalsIgnoreCase(id)) {
                return student;
            }
        }
        return null;
    }
}
```

### ⚙️ How to Run the Program
To compile and run this program, you need to have the Java Development Kit (JDK) installed on your computer.
1. **Save the Code**: Copy the code above and save it in a file named `StudentMgmtSystem.java`. Make sure the file name exactly matches the public class name.
2. **Open a Terminal/Command Prompt**: Navigate to the directory where you saved the files.
3. **Complie the Code**: Run the following command to compile the Java source file into bytecode.

```
javac StudentMgmtSystem.java
```

This will create two `.class` files: `Student.class` and `StudentMgmtSystem.class`. 

4. **Run the Program**: Execute the compiled code with the follwing command.
```
java StudentMgmtSystem
```
The Program will start, and you will see the administrator menu.

### 🖥️ How to Use the Administrator Interface
Once the program is running, you will be presented with a menu:
```
===== Student Record Management System =====
1. Add New Student
2. Update Student Information
3. View Student Details
4. Exit
==========================================
Enter your choice:
```
- To Add a Student (Option 1):
  - Enter `1` and press Enter.
  - The System will prompt you to enter the Student ID, Name, Age, and Grade.
  - Provide the required information for each prompt.
  - A success messgae will be displayed once the student is added.
<br>
- To Update a Student (Option 2):
  - Enter `2` and press Enter.
  - You will be asked for the ID of the student whose record you want to update.
  - If the student is found, their current details will be displayed.
  - You can then enter new information for the name, age, and grade. If you    want to keep the existing information for a field, simply press Enter without typing anything.
<br>
- To View Student Detail (Option 3):
  - Enter `3` and press Enter.
  - Provide the Student ID of the student you wish to view.
  - If the student exists, their complete details will be displayed in a formatted card.
<br>
- To Exit the Program (Option 4):
  - Enter `4` and press Enter to safely shut down the application.
  
### ⚠️ Error Handling
The system includes checks for common errors:
- **Invalid Menu Input**: If you enter a non-numeric value or a number outside the range of 1-4, an error message will appear, and the menu will be displayed again.
- **Student Not Found**: When updating or viewing details, if you enter an ID that does not exist in the records, the system will notify you that the student was not found.
- **Duplicate Student ID**: The system prevents you from adding a new student with an ID that is already in use.
- **Invalid Age Input**: When adding or updating, if you provide non-numeric or negative input for age, the system will show an error and prompt you again or keep the old value.

