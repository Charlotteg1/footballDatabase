# Football Matches - Tasks

Each of the questions/tasks below can be answered using a `SELECT` query. When you find a solution copy it into the code block under the question before pushing your solution to GitHub.

1) Find all the matches from 2017.

```sql

SELECT * FROM matches WHERE season=2017; 

```

2) Find all the matches featuring Barcelona.

```sql

SELECT * FROM matches WHERE hometeam='Barcelona' OR awayteam='Barcelona';

```

3) What are the names of the Scottish divisions included?

```sql

SELECT DISTINCT name FROM divisions WHERE country='Scotland';

```

4) Find the value of the `code` for the `Bundesliga` division. Use that code to find out how many matches Freiburg have played in that division. HINT: You will need to query both tables

```sql

SELECT count(*) as total_matches FROM matches WHERE division_code = (SELECT divisions.code FROM divisions WHERE divisions.name='Bundesliga') AND (awayteam='Freiburg' OR hometeam='Freiburg');
```

5) Find the teams which include the word "City" in their name. 

```sql

SELECT DISTINCT hometeam  FROM matches WHERE (LOWER(hometeam) LIKE LOWER('%City%'));

```

6) How many different teams have played in matches recorded in a French division?

```sql

SELECT count(DISTINCT hometeam) FROM matches WHERE division_code IN (SELECT divisions.code FROM divisions WHERE divisions.country='France');

```

7) Have Huddersfield played Swansea in any of the recorded matches?

```sql

SELECT id, hometeam, awayteam, ftr, season FROM matches WHERE awayteam IN ('Huddersfield','Swansea') AND hometeam IN ('Swansea','Huddersfield') ORDER BY season DESC;

```

8) How many draws were there in the `Eredivisie` between 2010 and 2015?

```sql

SELECT count(*) FROM matches WHERE division_code IN (SELECT divisions.code FROM divisions WHERE divisions.name='Eredivisie') AND ftr='D' AND season < 2016 AND season > 2009 ;

```

9) Select the matches played in the Premier League in order of total goals scored from highest to lowest. When two matches have the same total the match with more home goals should come first.

```sql

SELECT id, hometeam, awayteam, fthg,ftag, SUM(fthg+ftag) AS total_goals FROM matches WHERE division_code=(SELECT divisions.code FROM divisions WHERE divisions.name='Premier League') GROUP BY id, hometeam, awayteam, fthg,ftag ORDER BY total_goals DESC,fthg DESC;


```

10) In which division and which season were the most goals scored?

```sql

--SELECT season,division_code, MAX(mycount) AS total_goals FROM (SELECT division_code,season, SUM(fthg+ftag) as mycount FROM matches GROUP BY division_code, season order by division_code, --season DESC) AS mycount GROUP BY division_code, season ORDER BY total_goals DESC 
--FETCH FIRST 1 ROWS ONLY;

SELECT division_code,season, SUM(fthg+ftag) as total_goals FROM matches GROUP BY division_code, season order by total_goals DESC LIMIT 1;
```

### Useful Resources

- [Filtering results](https://www.w3schools.com/sql/sql_where.asp)
- [Ordering results](https://www.w3schools.com/sql/sql_orderby.asp)
- [Grouping results](https://www.w3schools.com/sql/sql_groupby.asp)