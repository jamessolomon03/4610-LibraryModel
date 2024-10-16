# MIST 4610 Group Project 1

## Team Name
4610Fa24Group7 - Library Data Model

## Authors

- Robin Rubchenko [@robinrubchenko](https://github.com/robinrubchenko/Library-Model.git)
- James Solomon [@jamessolomon03](https://github.com/jamessolomon03/librarymodel.git)
- Kylee Hobbs [@kylee-hobbs](https://github.com/kylee-hobbs/MIST4610-Project-1.git)
- Madeline Dodson [@mpd62417](https://github.com/mpd62417/MIST4610-Library.git)

## Scenario Description

Our relational database tracks the operations of library branches in the state of Georgia. The model surrounds the main entity of Books and catalogs individual physical copies of books. Information such as authors, genres, and suppliers, are also tracked. A single branch location has users, employees, and can put on events that authors may or may not attend. Library users have a membership to one branch, and we track their book checkouts, returns, event attendance, and various transactions. This project aims to model the operations of the library system and the relationships between different elements of the business. The tables in our database have been populated with sample data, and we have performed various SQL queries on this data to answer questions that we believe provide value to the library’s business.

## Data Model

Our model is designed to manage a public library system, supporting the storage of information related to books, authors, suppliers, users, events, employees, and branches.

The central entity is Books which tracks book title, publication date, status (indicating availability), author, publisher, supplier, and the branch where the copy is located. Each row in the table represents a single copy of a book.

Each author is linked to their respective books through a one-to-many relationship. With this in mind, we have assumed all of our books have only one author which limits us from storing books with multiple authors. The Authors entity tracks basic information including their name, date of birth, and nationality.

Each book has a listed supplier that stores information about their name and location, allowing us to keep track of who/where we purchased each copy from. This relationship is one-to-many, illustrating that we can have many book copies from the same supplier, but each copy can only have been purchased from one supplier.

Books are categorized into genres, and the many-to-many relationship between Books and Genres is managed through an associative entity called BookGenre. Within this associative entity, we are also tracking the genrePriority (i.e. Primary or Secondary) which denotes the genre ranking for each book. The Genres table simply tracks the genre titles.

Branches hold information such as name, location, phone number, hours of operation, and head of the branch. The one-to-one relationship between Employees and Branches allows us to track which employee, through their foreign key, is the head of the branch. In doing so, we removed the referential integrity constraint for employeeID in the Branches table. The second half of this connection is the one-to-many relationship between Branches and Employees, denoting that a branch can have many employees but, as we have assumed, an employee can only work at one branch. This is tracked by storing the branchID for each employee’s associated branch in the Employees table as a foreign key.

Within the Employees table, we are tracking each employee’s name, position, email, salary, branchID, as their supervisor through a recursive one-to-many relationship that self-references the employeeID as a foreign key called supervisorID. This supervisorID can be NULL as the head of the library system would not have a supervisor.

The LibraryUsers table holds membership details for each user, such as the start and end dates of their membership, their membership status (i.e. active or inactive), and the branch they are associated with. In our model we are assuming that each user can only be a member of one specific branch, and this is tracked through a one-to-many relationship with Branches through the branchID foreign key.

Additionally, we can track transactions such as fines, donations, book purchases, etc. in the Transactions table, recording the amount, type of transaction, and the user involved through a one-to-many relationship with LibraryUsers with userCardNumber as the foreign key.

Most importantly, and central to the library business, is the user’s ability to borrow books, which is tracked by the Checkouts table. This associative entity connects LibraryUsers and Books while also recording checkout date, due date, and return date. The foreign keys of bookID and userCardNumber form a composite primary key with the addition of checkoutDate. This composite primary key, and specifically the checkoutDate in DATETIME format, allows users to check out the same book multiple times and even permits the rare case of checking a book out, returning it, and then checking it out again all in the same day.

Lastly, each branch hosts events, and the Events table tracks the event title, date, location, any associated author, and the branchID through the foreign key created by the one-to-many relationship between Events and Branches. We are assuming in our model that if there is an author at these events, they’re the only one. We made this assumption with the idea that events are usually book signings or readings where the focus would be on one author. However, we also have events such as book club meetings that do not have an author involved, meaning the author column would be NULL in these instances.

The UserEvents associative entity links LibraryUsers to Events through a many-to-many relationship, keeping a record of registration and attendance (i.e. Attended or Registered). This is so that a user can attend many events and an event can have many users.

While our database model supports book management, employee and user tracking, event participation, and financial transactions, it does have limits. For example, we have no way of tracking book reviews or ratings, inter-branch transfers of books, or multiple authors on one book.

<img width="1055" alt="FinalDataModel" src="https://github.com/user-attachments/assets/13db3599-e122-4c2a-a82e-ba3034902406">

## Data Dictionary


## Queries

1. Query 1 finds the most popular genres by checkouts per branch including their average checkout duration and number of unique borrowers

<img width="1172" alt="Query 1" src="https://github.com/user-attachments/assets/f8d675dc-4289-432e-a298-e13134cffaa9">

- This query allows you to find the most popular book genres at each branch. This can help with gauging supply and demand, and better understanding the trends of their readers.

2. Query 2 calculates the average fine amount per User and list users with outstanding fines above the average of their branch

<img width="1148" alt="Query 2" src="https://github.com/user-attachments/assets/91ad74d3-1d9b-44c6-812a-536dd1ef0759">

- This query helps the library pinpoint users with significant fines, allowing for efficient targeting of reminders and collection efforts, which improves operational efficiency and financial accountability across branches.

3. Query 3 lists the Library Users and their home Branch for users who have not attended any events and have only 1 checkout



- This query allows the system to pinpoint which of the user(s) are the least active within the library. Given the user’s and their Branch’s information we can easily focus on direct advertising to spark their interests and get them involved.

4. Query 4 lists the percentage of books per branch out of total books across all branches 

<img width="965" alt="Query 4" src="https://github.com/user-attachments/assets/beda686a-3818-4283-a605-42cbf3f2058d">

- This query allows us to find what percent of total books are found in each library. This is important to identify which branches are lacking books in comparison to other branches, which can then be prioritized in following book orders from suppliers.

5. Query 5 identifies who has most overdue checkout, how many days overdue it is, and who it was



- This query allows us to pinpoint the longest a book was returned past due. This is important to understand which customer is likely to return books very late. This also helps libraries understand the latest they can expect current outstanding books to be returned.

6. Query 6 lists the number of supervisees each supervisor has



- This query allows us to figure out the number of employees each supervisor has in each branch. Libraries want to ensure supervisors have similar amounts of employees under them to prevent burnout or power imbalance amongst supervisors.

7. Query 7 lists our book inventory alphabetically 

<img width="1169" alt="Query 7" src="https://github.com/user-attachments/assets/4b0323eb-8061-4211-857e-a4c8e69f8fef">

- Having all books listed A-Z allows libraries to find books when only the title is known quickly.

8. Query 8 lists users who have outstanding fines of at least $3



- With most libraries, fine amounts accrue each day overdue, so theoretically fines over $3 would be almost a week late if we assume fine increments are $0.50. By finding which users have fines over this amount, we can prioritize them first in collection efforts.

9. Query 9 lists the books that are currently checked out

<img width="1160" alt="Query 9" src="https://github.com/user-attachments/assets/e1fdc873-6c06-4f97-bac1-5067d3296bde">

- It is important to know which books are currently checked out so we can know our current inventory.

10. Query 10 lists the most popular books from the ‘50s for library display



- Libraries often want to highlight specific genres, authors, or other categories for theme weeks or promotional purposes. A library may want to highlight books from a specific decade for example. This query helps us pinpoint books from the 50s, which we can then choose a few to put on display.


## Database Information

Server: ns_4610Fa24Group7

Our queries are stored within the server following the TP_Q# format.
