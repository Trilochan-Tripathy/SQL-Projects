            -- Question Set 1 - Easy
-- Who is the senior most employee based on job title?
Select * from employee  
order by levels DESC
limit 1 

-- Which countries have the most Invoices?
select count(*) as total_invoice,billing_country from invoice
group by billing_country
order by total_invoice DESC 

-- What are top 3 values of total invoice?
select top 3 total from invoice 
order by total DESC

-- Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals
select SUM(total) as total_invoice,billing_city from invoice 
group by billing_city
order by total_invoice DESC  

-- Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money
select c.customer_id,c.first_name,c.last_name,SUM(i.total) as total_spend
from customer c inner join invoice I
on c.customer_id = i.customer_id
group by c.customer_id,c.first_name,c.last_name 
order by total_spend DESC 

           -- Question Set 2 – Moderate
-- Write query to return the email, first name, last name, & Genre of all Rock Music listeners. Return your list ordered alphabetically by email starting with A
select c.email,c.first_name,c.last_name,g.
from customer c 
inner join invoice i on c.customer_id = i.customer_id
inner join invoiceLine l on l.invoiceId = i.invoiceId
inner join track t on t.TrackId = l.TrackId
inner join genre g on g.GenreId = t.GenreId 
where g.name like '%Rock%'
order by email ASC  

-- Let's invite the artists who have written the most rock music in our dataset. Write aquery that returns the Artist name and total track count of the top 10 rock bands
select TOP 10 b.name,b.artistId,count(b.artistId) as no_of_songs
from track t 
inner join album a on t.albumId = a.albumId
inner join artist b on b.artistId = a.artistId
inner join genre g on g.genreId = t.genreId
where g.name like '%Rock%'
group by b.name,b.artistId
order by no_of_songs DESC 


-- Return all the track names that have a song length longer than the average song length. Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first
select Name,Miliseconds
from Track
where Miliseconds > ( select avg(Miliseconds) from track) 
order by Miliseconds DESC 


     -- Question Set 3 – Advance
-- Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent
SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name, SUM(il.unit_price*il.quantity) AS amount_spent
FROM invoice i
JOIN customer c ON c.customer_id = i.customer_id
JOIN invoice_line il ON il.invoice_id = i.invoice_id
JOIN track t ON t.track_id = il.track_id
JOIN album alb ON alb.album_id = t.album_id
JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id
GROUP BY 1,2,3,4
ORDER BY 5 DESC; 

-- We want to find out the most popular music Genre for each country. We determine the most popular genre as the genre with the highest amount of purchases. Write a query that returns each country along with the top Genre. For countries where  the maximum number of purchases is shared return all Genres.
WITH popular_genre AS 
(
    SELECT COUNT(invoice_line.quantity) AS purchases, customer.country, genre.name, genre.genre_id, 
	ROW_NUMBER() OVER(PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity) DESC) AS RowNo 
    FROM invoice_line 
	JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id
	JOIN customer ON customer.customer_id = invoice.customer_id
	JOIN track ON track.track_id = invoice_line.track_id
	JOIN genre ON genre.genre_id = track.genre_id
	GROUP BY 2,3,4
	ORDER BY 2 ASC, 1 DESC
)
SELECT * FROM popular_genre WHERE RowNo <= 1 

-- Write a query that determines the customer that has spent the most on music for each country. Write a query that returns the country along with the top customer and how much they spent.  For countries where the top amount spent is shared, provide all customers who spent this amount.
WITH Customter_with_country AS (
		SELECT customer.customer_id,first_name,last_name,billing_country,SUM(total) AS total_spending,
	    ROW_NUMBER() OVER(PARTITION BY billing_country ORDER BY SUM(total) DESC) AS RowNo 
		FROM invoice
		JOIN customer ON customer.customer_id = invoice.customer_id
		GROUP BY 1,2,3,4
		ORDER BY 4 ASC,5 DESC)
SELECT * FROM Customter_with_country WHERE RowNo <= 1 






