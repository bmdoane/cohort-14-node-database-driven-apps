1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.

SELECT Customer.FirstName || " " || Customer.LastName AS "Full Name", Customer.CustomerId, Customer.Country 
FROM Customer
WHERE Customer.Country != "USA";

2. Provide a query only showing the Customers from Brazil.

SELECT Customer.FirstName || " " || Customer.LastName AS "Full Name", Customer.Country 
FROM Customer
WHERE Customer.Country = "Brazil";

3. Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.

SELECT Customer.FirstName || " " || Customer.LastName AS "Full Name", Invoice.InvoiceId, Invoice.InvoiceDate, Invoice.BillingCountry 
FROM Customer
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE Customer.Country = "Brazil";

4. Provide a query showing only the Employees who are Sales Agents.

SELECT Employee.FirstName || " " || Employee.LastName AS "Full Name", Employee.Title 
FROM Employee
WHERE Employee.Title = "Sales Support Agent";

5. Provide a query showing a unique list of billing countries from the Invoice table.

SELECT Invoice.BillingCountry AS "Billing Countries" 
FROM Invoice
GROUP BY Invoice.BillingCountry;

6. Provide a query showing the invoices of customers who are from Brazil.

SELECT Customer.FirstName || " " || Customer.LastName AS "Full Name", Invoice.InvoiceId, Customer.Country 
FROM Customer
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE Customer.Country = "Brazil";

7. Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.

SELECT Employee.FirstName || " " || Employee.LastName AS "Sales Agent", Invoice.* 
FROM Employee
JOIN Customer ON Employee.EmployeeId = Customer.SupportRepId
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
ORDER BY Employee.LastName ASC, Invoice.InvoiceId ASC;

8. Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.

SELECT Customer.FirstName || " " || Customer.LastName AS "Customer Name", Invoice.Total, Customer.Country, Employee.FirstName || " " || Employee.LastName AS "Sales Rep"
FROM Customer 
JOIN Invoice ON Customer.CustomerId and Invoice.CustomerId
JOIN Employee ON Customer.SupportRepId = Employee.EmployeeId
ORDER BY "Customer Name" ASC;

9. How many Invoices were there in 2009 and 2011? What are the respective total sales for each of those years?

SELECT COUNT(*)
FROM Invoice
WHERE InvoiceDate LIKE "%2009%" OR InvoiceDate LIKE "%2011%";

SELECT SUM(Total)
FROM Invoice
WHERE InvoiceDate LIKE "%2009%";

SELECT SUM(Total)
FROM Invoice
WHERE InvoiceDate LIKE "%2011%";

10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.

SELECT InvoiceId, COUNT(InvoiceId)
FROM InvoiceLine
WHERE InvoiceId = "37";

11. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: [GROUP BY](http://www.sqlite.org/lang_select.html#resultset)

SELECT InvoiceId, COUNT(InvoiceId) 
FROM InvoiceLine
GROUP BY InvoiceId;

12. Provide a query that includes the track name with each invoice line item.

SELECT Track.Name, InvoiceLine.*
FROM InvoiceLine INNER JOIN Track ON InvoiceLine.InvoiceLineId = Track.TrackId;

13. Provide a query that includes the purchased track name AND artist name with each invoice line item.

SELECT Track.Name, Artist.Name, InvoiceLine.*
FROM InvoiceLine INNER JOIN Track ON InvoiceLine.InvoiceLineId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId and Artist.ArtistId;

14. Provide a query that shows the # of invoices per country. HINT: [GROUP BY](http://www.sqlite.org/lang_select.html#resultset)

SELECT BillingCountry, COUNT(Total) 
FROM Invoice
GROUP BY BillingCountry;

15. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resultant table.

SELECT Playlist.Name, COUNT(PlaylistTrack.Trackid) AS "Track Count"
FROM Playlist
INNER JOIN PlaylistTrack ON PlaylistTrack.PlaylistId = Playlist.PlaylistId
GROUP BY Playlist.Name;

16. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.

SELECT Track.Name AS "Track Name", Album.Title AS "Album", MediaType.Name AS "Media Type", Genre.Name AS "Genre"
FROM Track
INNER JOIN Album ON Album.AlbumId = Track.AlbumId
INNER JOIN Genre ON Track.GenreId = Genre.GenreId
INNER JOIN MediaType ON Track.MediaTypeId = Genre.GenreId;

17. Provide a query that shows all Invoices but includes the # of invoice line items.

SELECT Invoice.*, COUNT(InvoiceLine.InvoiceLineId) AS "Line Item #'s"
FROM Invoice
INNER JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
GROUP BY Invoice.InvoiceId;

18. Provide a query that shows total sales made by each sales agent.

SELECT Employee.FirstName || " " || Employee.LastName AS "Sales Agent", SUM(Invoice.Total) AS "Total Sales"
FROM Invoice
INNER JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
INNER JOIN Employee ON Employee.EmployeeId = Customer.SupportRepId
GROUP BY Employee.EmployeeId;

19. Which sales agent made the most in sales in 2009?

SELECT "Sales Agent", MAX("Total Sales") AS "Highest Total Sales 2009"
FROM (
SELECT Employee.FirstName || " " || Employee.LastName AS "Sales Agent", SUM(Invoice.Total) AS "Total Sales"
FROM Employee
INNER JOIN Customer ON Customer.SupportRepId = Employee.EmployeeId
INNER JOIN Invoice ON Invoice.CustomerId = Customer.CustomerId
WHERE Invoice.InvoiceDate LIKE '%2009%'
GROUP BY Employee.LastName;

20. Which sales agent made the most in sales in 2010?

SELECT "Sales Agent", MAX("Total Sales") AS "Highest Total Sales 2010"
FROM (
SELECT Employee.FirstName || " " || Employee.LastName AS "Sales Agent", SUM(Invoice.Total) AS "Total Sales"
FROM Employee
INNER JOIN Customer ON Customer.SupportRepId = Employee.EmployeeId
INNER JOIN Invoice ON Invoice.CustomerId = Customer.CustomerId
WHERE Invoice.InvoiceDate LIKE '%2010%'
GROUP BY Employee.LastName;

21. Which sales agent made the most in sales over all?

SELECT Employee.FirstName || " " || Employee.LastName AS "Sales Agent", SUM(Invoice.Total) AS "Total Sales"
FROM Invoice
INNER JOIN Customer ON Invoice.CustomerId = Customer.CustomerId
INNER JOIN Employee ON Customer.SupportRepId = Employee.EmployeeId
GROUP BY Employee.EmployeeId LIMIT 1;

22. Provide a query that shows the # of customers assigned to each sales agent.

SELECT Employee.FirstName || " " || Employee.LastName AS "Sales Agent", COUNT(Customer.CustomerId) AS "Customers"
FROM Invoice
INNER JOIN Customer ON Customer.CustomerId = Invoice.CustomerId
INNER JOIN Employee ON Employee.EmployeeId = Customer.SupportRepId
GROUP BY Employee.EmployeeId;

23. Provide a query that shows the total sales per country. Which country's customers spent the most?

SELECT SUM(Invoice.Total) AS "Total Sales", Invoice.BillingCountry AS "Country"
FROM Invoice
GROUP BY Invoice.BillingCountry
ORDER BY "Total Sales" DESC;

24. Provide a query that shows the most purchased track of 2013.

SELECT Track.Name, COUNT(Track.Name) AS "Total"
FROM InvoiceLine
INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
INNER JOIN Invoice ON Invoice.InvoiceId = InvoiceLine.InvoiceId
WHERE Invoice.InvoiceDate LIKE "%2013%"
GROUP BY Track.Name
ORDER BY "Total"
DESC LIMIT 1;

25. Provide a query that shows the top 5 most purchased tracks over all.

SELECT Track.Name, COUNT(Track.Name) AS "Total"
FROM InvoiceLine
INNER JOIN Track ON Track.TrackId = InvoiceLine.TrackId
INNER JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
GROUP BY Track.Name
ORDER BY "Total"
DESC LIMIT 5;

26. Provide a query that shows the top 3 best selling artists.

SELECT Artist.Name, Sum(Invoice.Total) AS "Total" 
FROM Artist 
JOIN Album ON Artist.ArtistId = Album.ArtistId 
JOIN Track ON Album.AlbumId = Track.AlbumId 
JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId 
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId 
GROUP BY Artist.Name 
ORDER BY "Total" 
DESC Limit 3;

27. Provide a query that shows the most purchased Media Type.

SELECT MediaType.Name, SUM(Invoice.Total) AS "Total"
FROM MediaType 
JOIN Track ON MediaType.MediaTypeId = Track.MediaTypeId
JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
GROUP BY MediaType.Name
ORDER BY "Total"
DESC LIMIT 1;













