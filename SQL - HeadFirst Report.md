DATE: 2024-07-15
TIME: 21:44
STATE: #baby 
TAGS: [Work 1](Work%201.md) [Software-Development](./Software-Development.md) [Book](./Book.md) [SQL](./SQL.md)
# NOTE

SQL - BLOCK 14 

Chapter 6 
We're designing and creating a database for a video store to categorize the movie titles the store rents & sells. 

We will shelve these titles by genre in our `movie_db`.
		Here is a simple visualization of the `movie_db`

| movie_id | title | rating | genre/category | purchase/rent_date |
| -------- | ----- | ------ | -------------- | ------------------ |

1st Issue with this table is that we don't know where the titles should be shelved after they have been returned. (Because `movie_id` should only be associated with 1 `genre/category`)

	2nd Issue: None of the genres take procedure or precedence over one another (making it hard to know which genre the movie title pk(belongs to))
	 3rd Issue: Adding the True/False data values to each separate genre category for a single title is time consuming and error-prone. < which has been resolved in the above table >
	 4th Issue: `movie_id` with more than one `genre/category` 
		 Reason: the query for the category column works with an UPDATE having more than one Truth value that row makes that query result in: "?" (the result to the UPDATE expression will yield different or indeterminant since the value of True exists on more than one column in that row)
		 TO TACKLE THIS ISSUE WE USE THE `CASE` expression (which is a compound `UPDATE` expression)