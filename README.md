# Smart-Library-Management-System
import java.util.*;

class Book {
    private int id;
    private String title;
    private boolean isAvailable;
    private String borrowedBy;

    public Book(int id, String title) {
        this.id = id;
        this.title = title;
        this.isAvailable = true;
        this.borrowedBy = "None";
    }

    public int getId() { return id; }
    public String getTitle() { return title; }
    public boolean isAvailable() { return isAvailable; }
    public String getBorrowedBy() { return borrowedBy; }

    public void borrow(String userInfo) {
        isAvailable = false;
        borrowedBy = userInfo;
    }

    public void returnBook() {
        isAvailable = true;
        borrowedBy = "None";
    }
}

abstract class LibrarySystem {
    abstract void borrowBook(Book b);
    abstract void returnBook(Book b);
}

class User extends LibrarySystem {
    protected String name;
    protected String userId;

    public User(String name, String userId) {
        this.name = name;
        this.userId = userId;
    }

    void borrowBook(Book b) {}
    void returnBook(Book b) {}
}

class Student extends User {

    public Student(String name, String userId) {
        super(name, userId);
    }

    @Override
    void borrowBook(Book b) {
        if (b.isAvailable()) {
            b.borrow(name + " (ID: " + userId + ")");
            System.out.println(name + " borrowed (Student): " + b.getTitle());
        } else {
            System.out.println("Book not available!");
        }
    }

    @Override
    void returnBook(Book b) {
        if (!b.isAvailable()) {
            b.returnBook();
            System.out.println(name + " returned (Student): " + b.getTitle());
        } else {
            System.out.println("This book was not borrowed!");
        }
    }
}

class Teacher extends User {

    public Teacher(String name, String userId) {
        super(name, userId);
    }

    @Override
    void borrowBook(Book b) {
        if (b.isAvailable()) {
            b.borrow(name + " (ID: " + userId + ")");
            System.out.println(name + " borrowed (Teacher): " + b.getTitle());
        } else {
            System.out.println("Book not available!");
        }
    }

    @Override
    void returnBook(Book b) {
        if (!b.isAvailable()) {
            b.returnBook();
            System.out.println(name + " returned (Teacher): " + b.getTitle());
        } else {
            System.out.println("This book was not borrowed!");
        }
    }
}

public class LibraryDemo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        ArrayList<Book> books = new ArrayList<>();
        books.add(new Book(1, "Java Programming"));
        books.add(new Book(2, "Data Structures"));
        books.add(new Book(3, "OOP Concepts"));

        Student s1 = new Student("Promi", "242-35-006");
        Teacher t1 = new Teacher("Rahim Sir", "T-1001");

        while (true) {
            System.out.println("\n--- Library Menu ---");
            System.out.println("1. Show Books");
            System.out.println("2. Borrow Book (Student)");
            System.out.println("3. Borrow Book (Teacher)");
            System.out.println("4. Return Book (Student)");
            System.out.println("5. Return Book (Teacher)");
            System.out.println("6. Exit");

            int choice = sc.nextInt();

            System.out.print("Enter Book ID: ");
            int id = sc.nextInt();

            Book selectedBook = null;
            for (Book b : books) {
                if (b.getId() == id) {
                    selectedBook = b;
                    break;
                }
            }

            if (selectedBook == null) {
                System.out.println("Book not found!");
                continue;
            }

            switch (choice) {
                case 1:
                    for (Book b : books) {
                        System.out.println(b.getId() + " - " + b.getTitle()
                                + " | Available: " + b.isAvailable()
                                + " | Borrowed By: " + b.getBorrowedBy());
                    }
                    break;

                case 2:
                    s1.borrowBook(selectedBook);
                    break;

                case 3:
                    t1.borrowBook(selectedBook);
                    break;

                case 4:
                    s1.returnBook(selectedBook);
                    break;

                case 5:
                    t1.returnBook(selectedBook);
                    break;

                case 6:
                    System.out.println("Exiting...");
                    return;

                default:
                    System.out.println("Invalid choice!");
            }
        }
    }
}
