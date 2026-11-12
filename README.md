#include <iostream>
#include <fstream>
#include <iomanip>
using namespace std;

class Book
{
    int id;
    char title[50];
    char author[50];
    char student[50];
    int issued;

public:

    void addBook()
    {
        cout << "\nEnter Book ID: ";
        cin >> id;
        cin.ignore();

        cout << "Enter Book Title: ";
        cin.getline(title, 50);

        cout << "Enter Author Name: ";
        cin.getline(author, 50);

        issued = 0;
        student[0] = '\0';

        cout << "\nBook Added Successfully!\n";
    }

    void displayBook()
    {
        cout << "\nBook ID: " << id;
        cout << "\nTitle: " << title;
        cout << "\nAuthor: " << author;

        if (issued == 1)
        {
            cout << "\nStatus: Issued";
            cout << "\nIssued To: " << student;
        }
        else
        {
            cout << "\nStatus: Available";
        }

        cout << "\n-------------------------\n";
    }

    int getID()
    {
        return id;
    }

    void issueBook()
    {
        if (issued == 1)
        {
            cout << "\nBook is already issued!\n";
        }
        else
        {
            cin.ignore();
            cout << "Enter Student Name: ";
            cin.getline(student, 50);

            issued = 1;
            cout << "\nBook Issued Successfully!\n";
        }
    }

    void returnBook()
    {
        if (issued == 0)
        {
            cout << "\nBook is already available!\n";
        }
        else
        {
            issued = 0;
            student[0] = '\0';

            cout << "\nBook Returned Successfully!\n";
        }
    }

    void updateBook()
    {
        cin.ignore();

        cout << "Enter New Book Title: ";
        cin.getline(title, 50);

        cout << "Enter New Author Name: ";
        cin.getline(author, 50);

        cout << "\nBook Updated Successfully!\n";
    }
};

// Add a new book
void addNewBook()
{
    Book b;
    ofstream file("library.dat", ios::binary | ios::app);

    b.addBook();
    file.write((char*)&b, sizeof(b));

    file.close();
}

// Display all books
void displayAllBooks()
{
    Book b;
    ifstream file("library.dat", ios::binary);

    if (!file)
    {
        cout << "\nNo Records Found!\n";
        return;
    }

    while (file.read((char*)&b, sizeof(b)))
    {
        b.displayBook();
    }

    file.close();
}

// Search book by ID
void searchBook()
{
    Book b;
    int id, found = 0;

    cout << "Enter Book ID: ";
    cin >> id;

    ifstream file("library.dat", ios::binary);

    while (file.read((char*)&b, sizeof(b)))
    {
        if (b.getID() == id)
        {
            b.displayBook();
            found = 1;
            break;
        }
    }

    file.close();

    if (!found)
        cout << "\nBook Not Found!\n";
}

// Issue a book
void issueBook()
{
    Book b;
    int id, found = 0;

    cout << "Enter Book ID to Issue: ";
    cin >> id;

    fstream file("library.dat", ios::binary | ios::in | ios::out);

    while (file.read((char*)&b, sizeof(b)))
    {
        if (b.getID() == id)
        {
            b.issueBook();

            file.seekp(-((int)sizeof(b)), ios::cur);
            file.write((char*)&b, sizeof(b));

            found = 1;
            break;
        }
    }

    file.close();

    if (!found)
        cout << "\nBook Not Found!\n";
}

// Return a book
void returnBook()
{
    Book b;
    int id, found = 0;

    cout << "Enter Book ID to Return: ";
    cin >> id;

    fstream file("library.dat", ios::binary | ios::in | ios::out);

    while (file.read((char*)&b, sizeof(b)))
    {
        if (b.getID() == id)
        {
            b.returnBook();

            file.seekp(-((int)sizeof(b)), ios::cur);
            file.write((char*)&b, sizeof(b));

            found = 1;
            break;
        }
    }

    file.close();

    if (!found)
        cout << "\nBook Not Found!\n";
}

// Update book details
void updateBook()
{
    Book b;
    int id, found = 0;

    cout << "Enter Book ID to Update: ";
    cin >> id;

    fstream file("library.dat", ios::binary | ios::in | ios::out);

    while (file.read((char*)&b, sizeof(b)))
    {
        if (b.getID() == id)
        {
            b.updateBook();

            file.seekp(-((int)sizeof(b)), ios::cur);
            file.write((char*)&b, sizeof(b));

            found = 1;
            break;
        }
    }

    file.close();

    if (!found)
        cout << "\nBook Not Found!\n";
}

// Delete a book
void deleteBook()
{
    Book b;
    int id, found = 0;

    cout << "Enter Book ID to Delete: ";
    cin >> id;

    ifstream inFile("library.dat", ios::binary);
    ofstream outFile("temp.dat", ios::binary);

    while (inFile.read((char*)&b, sizeof(b)))
    {
        if (b.getID() == id)
        {
            found = 1;
        }
        else
        {
            outFile.write((char*)&b, sizeof(b));
        }
    }

    inFile.close();
    outFile.close();

    remove("library.dat");
    rename("temp.dat", "library.dat");

    if (found)
        cout << "\nBook Deleted Successfully!\n";
    else
        cout << "\nBook Not Found!\n";
}

// Main Function
int main()
{
    int choice;

    do
    {
        cout << "\n================================";
        cout << "\n     LIBRARY MANAGEMENT SYSTEM";
        cout << "\n================================";
        cout << "\n1. Add New Book";
        cout << "\n2. View All Books";
        cout << "\n3. Search Book";
        cout << "\n4. Issue Book";
        cout << "\n5. Return Book";
        cout << "\n6. Update Book";
        cout << "\n7. Delete Book";
        cout << "\n8. Exit";
        cout << "\n================================";

        cout << "\nEnter Your Choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            addNewBook();
            break;

        case 2:
            displayAllBooks();
            break;

        case 3:
            searchBook();
            break;

        case 4:
            issueBook();
            break;

        case 5:
            returnBook();
            break;

        case 6:
            updateBook();
            break;

        case 7:
            deleteBook();
            break;

        case 8:
            cout << "\nThank You for Using Library System!\n";
            break;

        default:
            cout << "\nInvalid Choice!\n";
        }

    } while (choice != 8);

    return 0;
}
