# 💥 👾 🎮 Twitch-Project 👾 🎮 💥

## Table of Contents

* [Solution](https://github.com/tanelam/Twitch-Project-#solution)
* [References](https://github.com/tanelam/Twitch-Project-#references)


## Solution

1. `SELECT` all columns from the first 20 rows of `stream` table.

```sql
SELECT *
FROM stream
LIMIT 20;
```
---

2. `SELECT` all columns from the first 20 rows of `chat` table.

```sql
SELECT *
FROM chat
LIMIT 20;
```
---

3. There is something wrong with the `chat` table. Its 1st row is actually the column names. Delete the first row of the `chat` table.

*First, I specified the table where I wanted to remove the row, in this case, the table is `chat`. Then, I added a search condition `WHERE` and I specified which row to remove. In this case, I used `WHERE time = 'time'` because the values of the column 'time' are numbers and it was easier to check against the "time" string and not delete any other by accident.*

```sql
DELETE
FROM chat
WHERE time = 'time'
```
---

4. What are the `DISTINCT` `game` in the `stream` table?

```sql
SELECT
DISTINCT game
FROM stream
```
---

5. What are the `DISTINCT` `channel`s in the `stream` table?

```sql
SELECT
DISTINCT channel
FROM stream
```
---

6. What are the most popular games in `stream`? Create a list of games and their number of viewers. `ORDER BY` from most popular to least popular.

*To get the most popular games in `stream`, I selected the 'game' column then I used the `count` aggregate function and passed 'game' as an argument, which returned the number of viewers of each game. I used `GROUP BY` which automatically sorts the returned values in alphabetical order, but leaves the empty cells or NULL on top of the list.*

```sql
SELECT game,
count(game)
FROM stream
GROUP BY game
```

*To order the list from most popular to least popular I used `ORDER BY` which by default sorts the returned values in ascending order. I used the `DESC` keyword, as below, to sort in descending order.*

```sql
SELECT game,
count(game)
FROM stream
GROUP BY game
ORDER BY count(game)
DESC;
```

  * *Just for fun, to get the top-ten most popular games in the `stream` table I used the `LIMIT` keyword at the end of the query to specify the number of records I wanted to have in return.*

```sql  
SELECT game,
count(game)
FROM stream
GROUP BY game
ORDER BY count(game)
DESC LIMIT 10;

```
---

7. There are some big numbers from the game `League of Legends` in `stream`. Where are these `League of Legend players` located?

    - **Hint:** Create a list.

*In this function, I created a list of countries where the game `League of Legends` is played. First, I selected the `game` and `country` columns from the `stream` table, then I added the `WHERE` clause to get the values that matched the `League of Legends` in the `game` column. Finally, I grouped them by country which automatically sorts the returned values in alphabetical order.*

```sql
SELECT game, country
FROM stream
WHERE game = 'League of Legends'  
GROUP BY country;
```
  * *Just for fun, I used the `count` aggregate function to have the number of players in each country.*

```sql
SELECT game, country,
count(country)
FROM stream
WHERE game = 'League of Legends'  
GROUP BY country;
```
  * *Then I sorted the countries from most to least players.*

```sql
SELECT game, country,
count(country)
FROM stream
WHERE game = 'League of Legends'  
GROUP BY country
ORDER BY count (country)
DESC;
```
---

8. The `player` column shows the source/device the viewer is using (site, iphone, android, etc). Create a list of players and their number of streamers.

*First, I created the list of the existing players/devices with its number of streamers using the `count` function and passed player as an argument. Then, I grouped them by `player`. I love to organize everything 🙈, so I sorted the return records from most to least number of streamers, using `ORDER BY` with the aggregate function `count` and the `DESC` keyword .*

```sql
SELECT player,
count(player)
FROM stream
GROUP BY player
ORDER BY count (player)
DESC;
```
---

9. Using a `CASE` statement, create a new column named `genre` for each of the games in `stream`. Group the games into their genres: Multiplayer Online Battle Arena (MOBA), First Person Shooter (FPS), and Others. Your logic should be: *If it is `League of Leagues` or `Dota 2` or `Heroes of the Storm` → then it is `MOBA`. If it is `Counter-Strike: Global Offensive` → then it is `FPS`. Else, it is `Others`.*

    - **Hint:** Use `GROUP BY` and `ORDER BY` to showcase only the unique game titles.

*In this function, I used `SELECT` to get all the columns in the `stream` table. Then I use the `CASE` statement to compare the case-expression, in this case `game`, with the expression in the `WHEN` clause. The values got saved into a new column called `genre`. As the instructions suggested, I used `GROUP BY` and `ORDER BY` to showcase only the unique game titles.*

```sql
SELECT *,
CASE game
  WHEN 'League of Legends' THEN 'MOBA'  
  WHEN 'Dota 2' THEN 'MOBA'  
  WHEN 'Heroes of the Storm' THEN 'MOBA'
  WHEN 'Counter-Strike: Global Offensive' THEN 'FPS'
  ELSE 'Others'
END
AS genre
FROM stream
GROUP BY game
ORDER BY genre  
```
---

10. The `stream` table and the `chat` table share a column: `device_id`. Do a `JOIN` of the two tables on that column.

*In this function, I used `INNER JOIN` to query data from the two tables, a table is associated with another table using a foreign key, in this case we have 'device_id'. Using `SELECT` keyword I selected the columns from the tables that I want to have in my join table. Also, I rename the column `device_id` from both tables to have a better picture of combination of data. The `INNER JOIN` clause returns rows from the `stream` table that has the corresponding row in the `chat` table.*

```sql
SELECT stream.game,
stream.country,
stream.player,
stream.channel,
stream.device_id as stream_device_id,
chat.device_id as chat_device_id
FROM stream
INNER JOIN chat
ON stream.device_id = chat.device_id;
```
**Bonus:** Now try to find some other interesting insights using SQL!

- Other solution for question #6

By using the asterisk as an argument of `count`, as below, it returns the number of rows in a table, including the rows that contain NULL values

```sql
SELECT game,
count(*)
FROM stream
GROUP BY game
ORDER BY count(*)
DESC;
```

## References

Here are the places where I looked and referenced to support my solutions.

- [SQLite Tutorial](http://www.sqlitetutorial.net/)

- [SQLite Org](https://www.sqlite.org/index.html)

- [SQL Clauses - Aggregate Functions](http://www.sqlclauses.com/sql+aggregate+functions)

- [w3schools - Order By](https://www.w3schools.com/sql/sql_orderby.asp)

- [GeeksForGeeks - Case Statement](https://www.geeksforgeeks.org/sql-case-statement/)

- [SQL Joins](https://www.w3schools.com/sql/sql_join.asp)
