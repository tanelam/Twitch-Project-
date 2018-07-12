# 💥 👾 🎮 Twitch-Project 👾 🎮 💥

 I used [DB Browser for SQLite](https://sqlitebrowser.org/) to complete this assignment, for its flexibility to manipulate database files compatible with SQLite.

## Table of Contents

* [Solution](https://github.com/tanelam/Twitch-Project-#solution)
* [Bonus](https://github.com/tanelam/Twitch-Project-#bonus)
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
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/2.jpg)

---

3. There is something wrong with the `chat` table. Its 1st row is actually the column names. Delete the first row of the `chat` table.

*First, I specified the table where I wanted to remove the row from, in this case, the table is `chat`. Then, I added a search condition `WHERE` and specified which row to remove. I used `WHERE time = 'time'` because the values in the column 'time' are in `datetime` format and it was easier to check against the "time" string.*

```sql
DELETE
FROM chat
WHERE time = 'time'
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/3.jpg)
---

4. What are the `DISTINCT` `game` in the `stream` table?

```sql
SELECT
DISTINCT game
FROM stream
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/4A.jpg)

![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/4B.jpg)
---

5. What are the `DISTINCT` `channel`s in the `stream` table?

```sql
SELECT
DISTINCT channel
FROM stream
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/5.jpg)
---

6. What are the most popular games in `stream`? Create a list of games and their number of viewers. `ORDER BY` from most popular to least popular.

*To get the most popular games in `stream`, I selected the 'game' column then I used the `count` aggregate function and passed 'game' as an argument, which returned the number of viewers of each game. I used `GROUP BY` which automatically sorts the returned values in alphabetical order, but leaves the empty cells or NULL on top of the list.*

```sql
SELECT game,
count(game)
FROM stream
GROUP BY game
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/6A.jpg)

*To organize the list from most popular to least popular I used `ORDER BY` which by default sorts the returned values in ascending order. I used the `DESC` keyword, as below, to sort in descending order.*

```sql
SELECT game,
count(game)
FROM stream
GROUP BY game
ORDER BY count(game)
DESC;
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/6B.jpg)

  * *Just for fun, to get the top-ten most popular games in the `stream` table I used the `LIMIT` keyword at the end of the query to specify the number of records I wanted to have in return.*

```sql  
SELECT game,
count(game)
FROM stream
GROUP BY game
ORDER BY count(game)
DESC LIMIT 10;

```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/6C.jpg)
---

- Other solution for question #6

By using the asterisk as an argument of `count`, as below, it returns the number of rows in a table, including the rows that contain NULL values.

```sql
SELECT game,
count(*)
FROM stream
GROUP BY game
ORDER BY count(*)
DESC;
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/Bonus.jpg)
---

7. There are some big numbers from the game `League of Legends` in `stream`. Where are these `League of Legend players` located?

    - **Hint:** Create a list.

*In this function, I created a list of countries where the game `League of Legends` is played. First, I selected the `game` and `country` columns from the `stream` table, then I added the `WHERE` clause to get the records that matched the `League of Legends` in the `game` column. Finally, I grouped them by country which automatically sorts the returned values in alphabetical order.*

```sql
SELECT game, country
FROM stream
WHERE game = 'League of Legends'  
GROUP BY country;
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/7A.jpg)

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
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/7C.jpg)
---

8. The `player` column shows the source/device the viewer is using (site, iphone, android, etc). Create a list of players and their number of streamers.

*In this function, I created a list of the existing players/devices each with their number of streamers using the `count` function and passed `player` as an argument. Then, I grouped them by `player`.*

```sql
SELECT player,
count(player)
FROM stream
GROUP BY player
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/8A.jpg)

* *I love to organize everything 🙈, so I sorted the return records from most to least number of streamers, using `ORDER BY` with the aggregate function `count` and the `DESC` keyword .*

```sql
SELECT player,
count(player)
FROM stream
GROUP BY player
ORDER BY count (player)
DESC;
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/8B.jpg)
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
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/9.jpg)
---

10. The `stream` table and the `chat` table share a column: `device_id`. Do a `JOIN` of the two tables on that column.

*In this function, I used `INNER JOIN` to link data between the two tables based on a common column between them 'device_id'. Using the `SELECT` keyword I specified the columns from the tables that I wanted to query data from. I used the table_name.column_name notation to grab columns from two different tables. Also, I rename the column `device_id` from both tables to have a better picture of combination of data. I join the tables with the `INNER JOIN` clause  which returns rows from the `stream` table that has the corresponding row in the `chat` table.*

```sql
SELECT
  stream.game,
  stream.country,
  stream.player,
  stream.channel,
  stream.device_id as stream_device_id,
  chat.device_id as chat_device_id
FROM stream
INNER JOIN chat
ON stream.device_id = chat.device_id;
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/10.jpg)

---
## Bonus

**Bonus:** Now try to find some other interesting insights using SQL!

- You can create your own table with `CREATE TABLE` keyword followed by the name of the table and passing as arguments the name of the columns and the datatype for each.

```sql
CREATE TABLE cats (
id INTEGER PRIMARY KEY,
name TEXT,
age INTEGER,
breed TEXT);
```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/Bonus2A.jpg)

- And add values by using the `INSERT INTO` and `VALUES` keywords.
```sql
INSERT INTO cats
  (name, age, breed)
VALUES
  ("Luca", 4, "Scottish Fold");

INSERT INTO cats
  (name, age, breed)
VALUES
  ("Tama", 8, "Grey Cat");

INSERT INTO cats
  (name, age, breed)
VALUES
  ("Kodi", 3, "White Cat");

INSERT INTO cats
  (name, age, breed)
VALUES
  ("Ukulele", 3, "Scottish Fold");

```
![alt text](https://github.com/tanelam/Twitch-Project-/blob/master/images/Bonus2B.jpg)

![alt text](http://readme-pics.s3.amazonaws.com/Lil_Bub_2013_(crop_for_thumb).jpg)
---

## References

Here are the places where I looked and referenced to support my solutions.

- [SQLite Tutorial](http://www.sqlitetutorial.net/)

- [SQLite Org](https://www.sqlite.org/index.html)

- [SQL Clauses - Aggregate Functions](http://www.sqlclauses.com/sql+aggregate+functions)

- [w3schools - Order By](https://www.w3schools.com/sql/sql_orderby.asp)

- [GeeksForGeeks - Case Statement](https://www.geeksforgeeks.org/sql-case-statement/)

- [SQL Joins](https://www.w3schools.com/sql/sql_join.asp)

- [BD Browser for SQLite](https://sqlitebrowser.org/)
