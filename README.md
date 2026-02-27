import java.util.*;
import java.io.*;

class Student {
    String name;
    int roll;
    String grade;

    Student(String name, int roll, String grade) {
        this.name = name;
        this.roll = roll;
        this.grade = grade;
    }

    public String toString() {
        return roll + "," + name + "," + grade;
    }
}

public class StudentManagementSystem {
    static ArrayList<Student> list = new ArrayList<>();
    static Scanner sc = new Scanner(System.in);
    static String fileName = "students.txt";

    public static void main(String[] args) {
        loadFromFile();
        int choice;

        do {
            System.out.println("\n1.Add Student");
            System.out.println("2.Edit Student");
            System.out.println("3.Remove Student");
            System.out.println("4.Search Student");
            System.out.println("5.Display All");
            System.out.println("6.Exit");
            System.out.print("Enter choice: ");
            choice = sc.nextInt();
            sc.nextLine();

            if (choice == 1) addStudent();
            else if (choice == 2) editStudent();
            else if (choice == 3) removeStudent();
            else if (choice == 4) searchStudent();
            else if (choice == 5) displayAll();
            else if (choice == 6) saveToFile();
            else System.out.println("Invalid choice");

        } while (choice != 6);
    }

    static void addStudent() {
        System.out.print("Enter roll number: ");
        int roll = sc.nextInt();
        sc.nextLine();
        System.out.print("Enter name: ");
        String name = sc.nextLine();
        System.out.print("Enter grade: ");
        String grade = sc.nextLine();

        if (name.isEmpty() || grade.isEmpty()) {
            System.out.println("Invalid data");
            return;
        }

        list.add(new Student(name, roll, grade));
        System.out.println("Student added");
    }

    static void editStudent() {
        System.out.print("Enter roll number: ");
        int roll = sc.nextInt();
        sc.nextLine();

        for (Student s : list) {
            if (s.roll == roll) {
                System.out.print("Enter new name: ");
                s.name = sc.nextLine();
                System.out.print("Enter new grade: ");
                s.grade = sc.nextLine();
                System.out.println("Updated");
                return;
            }
        }
        System.out.println("Student not found");
    }

    static void removeStudent() {
        System.out.print("Enter roll number: ");
        int roll = sc.nextInt();

        Iterator<Student> it = list.iterator();
        while (it.hasNext()) {
            Student s = it.next();
            if (s.roll == roll) {
                it.remove();
                System.out.println("Removed");
                return;
            }
        }
        System.out.println("Student not found");
    }

    static void searchStudent() {
        System.out.print("Enter roll number: ");
        int roll = sc.nextInt();

        for (Student s : list) {
            if (s.roll == roll) {
                System.out.println("Roll: " + s.roll);
                System.out.println("Name: " + s.name);
                System.out.println("Grade: " + s.grade);
                return;
            }
        }
        System.out.println("Student not found");
    }

    static void displayAll() {
        for (Student s : list) {
            System.out.println("Roll: " + s.roll + " Name: " + s.name + " Grade: " + s.grade);
        }
    }

    static void saveToFile() {
        try {
            PrintWriter pw = new PrintWriter(new FileWriter(fileName));
            for (Student s : list) {
                pw.println(s);
            }
            pw.close();
        } catch (Exception e) {
        }
    }

    static void loadFromFile() {
        try {
            File file = new File(fileName);
            if (!file.exists()) return;

            Scanner fileScanner = new Scanner(file);
            while (fileScanner.hasNextLine()) {
                String line = fileScanner.nextLine();
                String[] data = line.split(",");
                list.add(new Student(data[1], Integer.parseInt(data[0]), data[2]));
            }
            fileScanner.close();
        } catch (Exception e) {
        }
    }
}
