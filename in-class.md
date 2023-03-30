
# Queries to try in class



## Your first query

In the query console window, type the following and click "run"

```
select * from `covid-19-dimensions-ai.data.publications` 
limit 100
```


Note that SQL syntax is NOT case sensitive. The "limit" part is used to only retrieve 100 rows.

## Let's count how many rows there are

In the query console window, type the following and click "run"

```
select count(*) from `covid-19-dimensions-ai.data.publications
```

Can you spot the difference from the previous query? (except for the "limit" part)?

## Let's see how many of these papers have been cited by other papers

That is: how many times has it been listed as a reference by another paper?

In the query console window, type the following and click "run"

```
select count(*) from `covid-19-dimensions-ai.data.publications
where citations_count > 0
```

## Let's see how many of these papers are missing citation information

In the query console window, type the following and click "run"

```
select count(*) from `covid-19-dimensions-ai.data.publications
where citations_count is null
```

Can you think of a reason why it's useful to distinguish "null" citations from 0 citations?


## Let's see what the min, max, and average citation counts are

We can get multiple results in one query.

In the query console window, type the following and click "run"

```
select min(citations_count) as minimum, avg(citations_count) as mean, max(citations_count) as maximum from `covid-19-dimensions-ai.data.publications`
```

We use aliasing with "as" to create meaningful column names in the output, instead of the default "f0_", "f1_", etc.

## Find the article with the highest citation count

In the query console window, type the following and click "run"

```
select title, doi, publisher.name, year from `covid-19-dimensions-ai.data.publications`
where citations_count >= 35240
```
">=" means "greater or equal to".

## Let's create a frequency distribution of citation counts

To count how many times different citation counts appear, type the following and click "run"

```
select citations_count, count(id) as n_publications from `covid-19-dimensions-ai.data.publications` 
group by citations_count
order by citations_count desc
```

"Group by" is required to get counts for each citation value.
"Order by" is not strictly necessary, but makes it easier to view the output.
Note that "desc" means "descending". The opposite ordering is "asc" (ascending).

## Let's download the results for analysis or visualisation

* Run the frequency distribution query above. 
* In the panel between the code query window and the results, click the button called "Save results"
* Select "CSV (local file)" to save the results to your computer.
* Save it, and open it with another programme (e.g. a spreadsheet application or R).

## Let's see when these articles were published

Type the following and click "run":

```
select year, count(id) as n from `covid-19-dimensions-ai.data.publications`
group by year
order by n desc
```

## Let's see which publishers are represented in the dataset

Note: "publisher" is a complex data field, and we use the dot-notation seen below to get just the name as a string.

type the following and click "run"

```
select publisher.name, count(id) as n_publications from `covid-19-dimensions-ai.data.publications` 
group by publisher.name
order by n_publications desc
```

What are the names of the top 3 publishers?


# Advanced examples

These examples use some more advanced syntax to show further possibilities for manipulating data with SQL in BigQuery. Specifically, they deal with arrays, which is the way that BigQuery handles multiple instances of the same kind of data per row. For instance, a paper will often have more than one author, and hence be linked to more than one country. However, instead of repeating the information about the paper over multiple rows (one row per author), BigQuery allows you to store the information in an array, inside a cell. We can use some special functions to deal with this.

## Let's count the average number of authors per paper

Try the following:

```
select avg(array_length(researcher_ids)) from `covid-19-dimensions-ai.data.publications`
```

In this example the `array_length` function returns the number of items (i.e. authors) in the "researcher_ids" column. Since this is a number we can directly apply the `avg` function to it.

## Let's find the top funding countries

Try the following:

``` 
select fc, count(fc) as n from `covid-19-dimensions-ai.data.publications` 
left join unnest(funder_countries) as fc
group by fc
order by n desc
limit 5
```

In this example the `left join unnest` statement untangles the array containing all the countries that have funded the research in a paper. In other words, we get a "flat" representation where the information about a paper is duplicated for each funding country that contributed. For that reason, we just count the occurrences of the funding countries instead of the document id (since it would be duplicated: alternatively, count document ids but use `count(distinct(id))` instead).

## Running multiple queries in one query window (as shown in class)

With Google BigQuery, you can execute multiple sequential queries by adding a semi-colon after each query. In the web console, you can click to view the output you want to see. For example:

```
select * from `covid-19-dimensions-ai.data.publications` 
limit 100;
select count(*) from `covid-19-dimensions-ai.data.publications
```

## Commenting your code (as shown in class)

There are two ways to comment out code in BigQuery SQL:

* two hyphens (--) for a single line
* a forwards slash followed by a star (opening) and then the reverse (closing) for multiple lines

Examples:

```
select * from `covid-19-dimensions-ai.data.publications`  -- this is a comment on a single line
limit 100 /* this comment...
... spans multiple lines */
```
## Sub-queries (as shown in class)

You can nest queries together in SQL to get more efficient code. Consider the example above to identify the article with the highest number of citations:

```
select title, doi, publisher.name, year from `covid-19-dimensions-ai.data.publications`
where citations_count >= 35240
```

In this case, there `where` condition uses a hard-coded citation count (the maximum at the present time). But what if that number changes? A more efficient and elegant way to do this is to use a sub-query, where we establish the filtering condition (i.e. what is the max) via another query, like this:

```
select title, doi, publisher.name, year from `covid-19-dimensions-ai.data.publications`
where citations_count = (
 select max(citations_count) from `covid-19-dimensions-ai.data.publications`
 )
 ```
 
 In the new code version above, the query will always return the correct result even when the underlying data changes. Also, we don't have to use `>=`, a simple `=` is sufficient because the sub-query will always return the max value.
 
 Note that the sub-query needs to be enclosed in round brackets.
```
