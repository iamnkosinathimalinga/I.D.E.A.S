Date: 2024-07-15
Time: 10:04
State: #baby 
Tags: [Software-Development](./Software-Development.md) [WORK TEMP](./WORK%20TEMP.md) [SQL](./SQL.md) [Work 1](Work%201.md)

# NOTES

![[httpatomoreillycomsourceoreillyimages1373299.png.jpg]]

# Populating the new column

Now we can translate those sentences into SQL

`UPDATE` statement:

Fill in the category value for these movies.

**`movie_table`**

|title|rating|drama|comedy|action|gore|scifi|for_kids|cartoon|category|
|---|---|---|---|---|---|---|---|---|---|
|Big Adventure|G|F|F|F|F|F|T|F||
|Greg: The Untold Story|PG|F|F|T|F|F|F|F||
|Mad Clowns|R|F|F|F|T|F|F|F||
|Paraskavedekatriaphobia|R|T|T|T|F|T|F|F||
|Rat named Darcy, A|G|F|F|F|F|F|T|F||
|End of the Line|R|T|F|F|T|T|F|T||
|Shiny Things, The|PG|T|F|F|F|F|F|F||
|Take it Back|R|F|T|F|F|F|F|F||
|Shark Bait|G|F|F|F|F|F|T|F||
|Angry Pirate|PG|F|T|F|F|F|F|T||
|Potentially Habitable Planet|PG|F|T|F|F|T|F|F||

**Does the order in which we evaluate each of the T/F columns matter?** 
		The order matters because if it didn't then a title which has true on more than one column would automatically be assigned the value in the last column which contains 'T'.
			>So our query has to match this logic, this is one of the issues which is addressed below:


Fill in the category value for these movies.

**`movie_table`**

![image with no caption](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780596526849/files/httpatomoreillycomsourceoreillyimages1373301.png.jpg)

##### Now we have an issue which is returning a "?" as the value in our category column:

To address the issue stated on the bottom of the table above, we can: 

Solution 1: (Which works partially but fails when we scale.) Have Two UPDATE statements which may change the same column’s value.

![image with no caption](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780596526849/files/httpatomoreillycomsourceoreillyimages1373303.png.jpg)

Or Solution 2: Writing one big UPDATE statement (But why do that when there's a better way? hahaha)

Implemented Solution: Using the `CASE` expression which combines all the `UPDATE` statements by: 1st-(checking an existing column’s value against a condition) then If the condition is met => the new column is filled with a specified value.

#### Here's the Updated **`movie_table`**

It even allows you to tell your RDBMS _**what to do if any records don’t meet the conditions**_

Here’s its basic syntax:

![image with no caption](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780596526849/files/httpatomoreillycomsourceoreillyimages1373305.png.jpg)

###### UPDATE with a CASE expression

Let’s see the `CASE` expression in action on our `movie_table`.

![image with no caption](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780596526849/files/httpatomoreillycomsourceoreillyimages1373307.png.jpg)

![image with no caption](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780596526849/files/httpatomoreillycomsourceoreillyimages1373309.png.jpg)

As each movie title’s T/F values are run through the `CASE` statement, the RDBMS is looking for the first ’T’ to set the category for each film.

Here’s what happens when ’Big Adventure’ runs through the code:

![image with no caption](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780596526849/files/httpatomoreillycomsourceoreillyimages1373311.png.jpg)


# 16 Jul 24

^ebf893


> [!NOTE] SQL - Progress Update
> < Contents in physical note book. Will add them and applicable links >
> What I learnt in chapter 6? Update-Case expressions, Distinct func, also understanding how results are yielded when i run a query, and the difference between a query and a expression. 
> MIN(),MAX(),AVG()
> You're not limited to ordering by one column, also learnt the DESC and DESCRIPTION and how context is important.
> 		 i.e. `SELECT first_name, SUM(sales) FROM cookie_table ORDER BY SUM(sales) GROUP BY first_name;
> 		 i.e. Using DESC (Descending in this context) : `SELECT first_name, SUM(DISTINCT sale_date) FROM cookie_table ORDER BY SUM(DISTINCT sale_date) GROUP BY first_name;`


