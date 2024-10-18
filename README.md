# MIST 4610 Group Project 1

## Team Name
4610Fa24Group7 - Library Data Model

## Team Members

- Robin Rubchenko [@robinrubchenko](https://github.com/robinrubchenko)
- James Solomon [@jamessolomon03](https://github.com/jamessolomon03)
- Kylee Hobbs [@kylee-hobbs](https://github.com/kylee-hobbs)
- Madeline Dodson [@mpd62417](https://github.com/mpd62417)

## Scenario Description

Our relational database tracks the operations of public library branches in the state of Georgia. The model surrounds the main entity of Books and catalogs individual physical copies of books. Information such as authors, genres, and suppliers, are also tracked. A single branch location has users, employees, and can put on events that authors may or may not attend. Library users have a membership to one branch, and we track their book checkouts, returns, event attendance, and various transactions. This project aims to model the operations of the library system and the relationships between different elements of the business. The tables in our database have been populated with sample data, and we have performed various SQL queries on this data to answer questions that we believe provide value to the library’s business.

## Data Model

<img width="1055" alt="FinalDataModel" src="https://github.com/user-attachments/assets/13db3599-e122-4c2a-a82e-ba3034902406">

## Data Model Explanation

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


## Data Dictionary

<img width="802" alt="Authors" src="https://github.com/user-attachments/assets/98e57ed2-0074-4367-b324-a070bb6280cd">
<img width="801" alt="BookGenre" src="https://github.com/user-attachments/assets/5fcff0a8-35ed-47a5-95d1-32a0386efb90">
<img width="811" alt="Books" src="https://github.com/user-attachments/assets/66524a20-3106-436c-999a-7fa337fc3e68">
<img width="805" alt="Branches" src="https://github.com/user-attachments/assets/53659011-dd2d-42eb-a9e6-8b5d76b61a31">
<img width="813" alt="Checkouts" src="https://github.com/user-attachments/assets/18b881f2-2490-4a66-9080-bb1a85b98917">
<img width="813" alt="Employees" src="https://github.com/user-attachments/assets/6ab365df-cdee-488f-b507-f683b4ee4a8d">
<img width="814" alt="Events" src="https://github.com/user-attachments/assets/f1f6368c-b983-4b6e-bebb-7d6d024ad202">
<img width="798" alt="Genre" src="https://github.com/user-attachments/assets/b7f28e1a-b6f4-40b5-bb0d-87a37461ea95">
<img width="813" alt="LibraryUsers" src="https://github.com/user-attachments/assets/b36f9eb6-fa6e-468e-9326-da511ef8e505">
<img width="798" alt="Suppliers" src="https://github.com/user-attachments/assets/98d8dcb9-1c59-454d-9e49-ad5af55a0aed">
<img width="814" alt="Transactions" src="https://github.com/user-attachments/assets/661f9062-678a-45ff-8a02-81a0fcb47fa4">
<img width="812" alt="UserEvents" src="https://github.com/user-attachments/assets/7d321dc6-ddb9-4e26-ba74-2d1e3322aac4">

# Queries

<img width="676" alt="Query Info" src="https://github.com/user-attachments/assets/011a86b7-cdab-4160-8499-68ee5f623297"> <br />

## Six Complex Queries

**1. Query 1 finds the most popular genres by checkout, per branch, including their average checkout duration and number of unique borrowers. The results are first ordered by the branch name and then by the number of checkouts in descending order.** <br />

<img width="1172" alt="Query 1" src="https://github.com/user-attachments/assets/14cb0117-e943-4710-9470-2735c5727307"><br />

- Query 1 allows the library to find the most popular book genres at each branch. This can help with gauging supply and demand and enables the library to better understand the trends of the readers at each branch.<br />


**2. Query 2 calculates the average fine amount per user, and lists information about the users with outstanding fines greater than the average fine amount for their branch. The information listed includes the user's name, their branch’s name and ID, their average outstanding fine, and the average fine for their branch as a whole.** <br />

<img width="1148" alt="Query 2" src="https://github.com/user-attachments/assets/8d3ae131-10da-46ba-98a4-66090ceaeab7"><br />

- Query 2 helps the library pinpoint users with outstanding fines that are above the average fine for their branch. This allows the library to efficiently target these users with reminders and collection efforts which improves operational efficiency and financial accountability across branches. The branches can also use this information to determine the riskiness of certain users in the future.<br />


**3. Query 3 lists the user’s card number, name, their branch’s name, and their branch’s ID for users who have not attended any events and only have 1 checkout.** <br />

<img width="931" alt="Query 3" src="https://github.com/user-attachments/assets/e0a951d4-1ddc-4dde-b265-a547724cd80d"><br />

- Query 3 allows the system to pinpoint which of the user(s) are the least active within the library, given that they do not participate in the library’s event offerings and have the minimum number of checkouts. Given their card number, name, and their branch’s name and ID, we can easily focus on direct advertising to spark their interest.<br />


**4. Query 4 lists each branch’s name and the percentage of books per branch out of total books in the library system.** <br />

<img width="943" alt="Query 4" src="https://github.com/user-attachments/assets/92f6e094-d307-421f-80ee-ddbb998d42bf"><br />

- Query 4 helps the library identify which branches are lacking book inventory in comparison to the other branches. The information provided can then be used to determine which branches need to order books from suppliers.<br />


**5. Query 5 identifies which user had the most overdue checkout and how many days overdue this checkout was, of the books returned.** <br />

<img width="934" alt="Query 5" src="https://github.com/user-attachments/assets/bc08423f-faa0-4e35-bae1-722a247310c3"><br />

- Query 5 allows the library system to pinpoint the longest a book was returned past due. This is important to understand which customers are likely to return books abnormally late but also provides an estimate of the latest the library can expect outstanding books to be returned. If a book is not returned by the maximum estimate, then the library can push reminders to the user or assume that the book may never be returned, allowing them to calculate their loss and take next steps. Additionally, given the name of the user who had the latest return, the library can focus its efforts on this medium-risk user. Although the user did return their book late, it was at least returned showing that this user is still engaged with the services and isn’t most efficient in using them. The library could then take steps to encourage this user to be more proactive and return books on time in the future.<br />


**6. Query 6 lists each supervisor’s name and the number of supervisees they each have.** <br />

<img width="951" alt="Query 6" src="https://github.com/user-attachments/assets/d3d4f942-73f1-49ac-b7f1-7aa80f7bdf57"><br />

- Query 6 allows the library system to figure out the number of employees each supervisor has in each branch. This is important to the library’s operations as it wants to ensure supervisors have similar amounts of employees under them to prevent burnout or power imbalance amongst supervisors. The supervisor with the most supervisees is listed first, with the rest following in descending order.<br />


## Four Simple Queries

**7. Query 7 lists the book inventory alphabetically for the library system as a whole.** <br />

<img width="1169" alt="Query 7" src="https://github.com/user-attachments/assets/bc00d063-a7e0-4a4b-b323-26be9b0d4496"><br />

- Query 7 lists the titles of all books in the library system’s inventory in alphabetical order. This allows the library to easily pinpoint books by their title, enabling them to notice gaps in their offerings and act accordingly.<br />


**8. Query 8 lists the first name of library users who have outstanding fines of at least $3.** <br />

<img width="743" alt="Query 8" src="https://github.com/user-attachments/assets/ff4a2028-a83a-400a-8139-e4cd02f793d5"><br />

- Our library system applies a $0.20 fine to checkouts every day they are past due, meaning a $3 fine distinguishes a checkout that is 15 days past due. Query 8 gives us the names of the library users that have outstanding fines of at least $3, meaning these users are the most risky to our business. Since query 5 told us the latest a book has ever been returned was 3 days past due, the library can assume these checkouts will likely never be returned. Given that these users are extremely risky, the library will also put their names on a list of users prohibited from making more checkouts as long as this checkout remains outstanding. The library may also decide to target these users heavily to get the checkouts returned, but this will be up to each branch individually because some may view this as a waste of resources due to the high probability of no return.<br />


**9. Query 9 lists the titles of books that are currently checked out, along with the first and last name of the library user who has the book checked out, and the corresponding checkout and due date.** <br />

<img width="1160" alt="Query 9" src="https://github.com/user-attachments/assets/f46feeef-502c-4745-9683-4040771f874b"><br />


- Query 9 is essential to inform the library of which books are currently checked, that way the library can keep track of the current inventory. Along with this, if there is a book that is currently checked out, but there is another library user who is interested in checking out the same book, having the due date can help the library keep this interested user updated on the status of the expected date they can checkout the book next.<br />


**10. Query 10 lists the title of the most popular books from the ‘50s along with the number of times this book has been checked out.** <br />

<img width="1175" alt="Query 10" src="https://github.com/user-attachments/assets/0e4fd15c-6f29-4a49-9703-6d332e789cdb"><br />

- Query 10 helps the library determine which books from this period should be displayed in the library to garner attention. Since libraries often want to highlight specific genres, authors, or other categories for theme weeks or promotional purposes, it is vital to know which books related to highlight are the most popular to bring in the most customers. In this specific case, our library wants to highlight books from the 50s, and the query orders the book titles by the number of checkouts in descending order, putting the most popular book from this period at the top, making it easy to pinpoint top choices.<br />

## Presentation File

[10:2:24_ProjectPresentation.pdf](https://github.com/user-attachments/files/17439801/10.2.24_ProjectPresentation.pdf)

## Database Information

Server: ns_4610Fa24Group7

Our queries are stored within the server following the TP_Q# format.
