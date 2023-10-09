# Library Management System

by Anthony Garcia

Course: Software Development 1

CRN: CEN 3024C

## SLDC Plan
[Module 2 SLDC Assignment FINAL.pdf](https://github.com/ToeKnee013/Garcia_Anthony_LMS/files/12569939/Module.2.SLDC.Assignment.FINAL.pdf)

## Text Files
[Book.txt](https://github.com/ToeKnee013/Garcia_Anthony_LMS/files/12569994/Book.txt)

[LibraryManagementSystem.txt](https://github.com/ToeKnee013/Garcia_Anthony_LMS/files/12569993/LibraryManagementSystem.txt)

[library.txt](https://github.com/ToeKnee013/Garcia_Anthony_LMS/files/12569992/library.txt)



## Raw Code

/**
 * Anthony Garcia
 * CEN 3024 - Software Development 1
 * September 10, 2023
 * LibraryManagementSystem.java
 * This console based application will do the following:
 * 1. add a book - Add a book to a text file with an ID, book name, and author
 * 2. remove a book - removes a book based on the selected ID
 * 3. list all books - List all books within the text file
 * 4. Check out a book
 * 5. Check in a book
 * 6. Quit - Exits the program
*/

package module2;

// Import Packages
import java.io.*;
import java.util.*;

//LMS Class
public class LibraryManagementSystem {
    private List<Book> books;
    //private static final String FILENAME = "C:\\Users\\ozore\\OneDrive\\Documents\\Software Development 1\\library.txt"; // Name of the text file
    static Scanner scanner = new Scanner(System.in);
    private static final String FILENAME = "C:\\Users\\ozore\\OneDrive\\Documents\\Software Development 1\\" + scanner.nextLine() + ".txt"; // Name of the text file


    public LibraryManagementSystem() {
        books = new ArrayList<>();
        loadLibraryFromFile(); // Load existing data from the file, if any
    }

    // Add a book object
    public void addBook(int id, String title, String author) {
        Book book = new Book(id, title, author);
        books.add(book);
        saveLibraryToFile(); // Save the updated collection to the file
    }

    // Remove a book object
    public void removeBook(int id) {
        Iterator<Book> iterator = books.iterator();
        while (iterator.hasNext()) {
            Book book = iterator.next();
            if (book.getId() == id) {
                iterator.remove();
                System.out.println("Book with ID " + id + " removed.");
                saveLibraryToFile(); // Save the updated collection to the file
                return;
            }
        }
        System.out.println("Book with ID " + id + " not found.");
    }

    // List all books object
    public void listAllBooks() {
        if (books.isEmpty()) {
            System.out.println("No books in the collection.");
        } else {
            System.out.println("Printing books in the collection:");
            for (Book book : books) {
                System.out.println(book);
            }
        }
    }

    // Write to text file object
    private void saveLibraryToFile() {
        try (PrintWriter writer = new PrintWriter(new FileWriter(FILENAME))) {
            for (Book book : books) {
                writer.println(book.toFileString());
            }
            System.out.println("Library data saved to " + FILENAME);
        } catch (IOException e) {
            System.err.println("Error saving library data to " + FILENAME);
            e.printStackTrace();
        }
    }
    
    // Read from text file object
    private void loadLibraryFromFile() {
        try (BufferedReader reader = new BufferedReader(new FileReader(FILENAME))) {
            String line;
            while ((line = reader.readLine()) != null) {
                Book book = Book.fromFileString(line);
                books.add(book);
            }
            System.out.println("Library data loaded from " + FILENAME);
        } catch (FileNotFoundException e) {
            System.out.println(FILENAME + " not found. Starting with an empty library.");
        } catch (IOException e) {
            System.err.println("Error loading library data from " + FILENAME);
            e.printStackTrace();
        }
    }

    
    
    public boolean checkOutBook(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title) && !book.isCheckedOut()) {
                book.checkOut();
                return true;
            }
        }
        return false;
    }

    public boolean checkInBook(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title) && book.isCheckedOut()) {
                book.checkIn();
                return true;
            }
        }
        return false;
    }
    
    
    // Main
    public static void main(String[] args) {
        LibraryManagementSystem library = new LibraryManagementSystem();
        Scanner scanner = new Scanner(System.in);

        //  while program is running, do the following
        while (true) {
            System.out.println("\nLibrary Management System Menu:");
            System.out.println("1. Add a book");
            System.out.println("2. Remove a book by ID");
            System.out.println("3. List all books");
            System.out.println("4. Check out a book");
            System.out.println("5. Check in a book");
            System.out.println("6. Quit");
            System.out.print("Enter your choice: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume the newline character

            // Switch case statements for choosing options
            switch (choice) {
            
            	// Add a book
                case 1:
                    System.out.print("Enter book ID: ");
                    int id = scanner.nextInt();
                    scanner.nextLine(); // Consume the newline character
                    System.out.print("Enter book title: ");
                    String title = scanner.nextLine();
                    System.out.print("Enter book author: ");
                    String author = scanner.nextLine();
                    library.addBook(id, title, author);
                    System.out.println("Book added to the collection.");
                    break;
                // Remove a book
                case 2:
                    System.out.print("Enter the ID of the book to remove: ");
                    int removeId = scanner.nextInt();
                    library.removeBook(removeId);
                    break;
                // List all books
                case 3:
                    library.listAllBooks();
                    break;
                // Check out book
                    
                case 4:
                // Ask the user for a title to check out
                    System.out.print("Enter a title to check out: ");
                    String input = scanner.nextLine();
                    boolean checkedOut = library.checkOutBook(input);

                    if (checkedOut) {
                        System.out.println("Book with title '" + input + "' checked out.");
                    } else {
                        System.out.println("Book with title '" + input + "' not found or already checked out.");
                    }
                    
                    case 5:
                    // Ask the user for a title to check in
                    System.out.print("Enter a title to check in: ");
                    input = scanner.nextLine();
                    boolean checkedIn = library.checkInBook(input);

                    if (checkedIn) {
                        System.out.println("Book with title '" + input + "' checked in.");
                    } else {
                        System.out.println("Book with title '" + input + "' not found or already checked in.");
                    }

                    // Exit program
                    case 6:
                        System.out.println("Exiting the Library Management System.");
                        System.exit(0);
                // Invalid choice if options 1-4 are not selected
                default:
                    System.out.println("Invalid choice. Please try again.");
                    scanner.close();
                    break;
        }
    }
}
}



/**
 * Anthony Garcia
 * CEN 3024 - Software Development 1
 * September 10, 2023
 * Book.java
 * This console based application will store the ID, title, and author. It will also convert a book object to a formatted string.
*/

package module2;

// Book class creation
class Book {
    private int id;
    private String title;
    private String author;
    private boolean checkedOut;


    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.checkedOut = false;
    }
    
    // Get the ID
    public int getId() {
        return id;
    }
    
 // Get the title
    public String getTitle() {
        return title;
    }

 // Get the author
    public String getAuthor() {
        return author;
    }

    // Concatenation of strings an objects
    @Override
    public String toString() {
        return "ID: " + id + ", Title: " + title + ", Author: " + author;
    }

    // Convert a Book object to a formatted string for writing to a file
    public String toFileString() {
        return id + "," + title + "," + author;
    }
    
    public boolean isCheckedOut() {
        return checkedOut;
    }

    public void checkOut() {
        checkedOut = true;
    }

    public void checkIn() {
        checkedOut = false;
    }

    // Create a Book object from the previously formatted string
    public static Book fromFileString(String line) {
        String[] parts = line.split(",");
        int id = Integer.parseInt(parts[0]);
        String title = parts[1];
        String author = parts[2];
        return new Book(id, title, author);
    }
}


