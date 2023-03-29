
# Queries to try in class



## Your first query

In the query console window, type the following and click "run"

``select * from `covid-19-dimensions-ai.data.publications` 
limit 100``

Note that SQL syntax is NOT case sensitive. The "limit" part is used to only retrieve 100 rows.

## Let's count how many rows there are

In the query console window, type the following and click "run"

``select count(*) from `covid-19-dimensions-ai.data.publications``

Can you spot the difference from the previous query? (except for the "limit" part)?

## Let's see how many of these papers have been cited by other papers

That is: how many times has it been listed as a reference by another paper?

In the query console window, type the following and click "run"

``select count(*) from `covid-19-dimensions-ai.data.publications
where citations_count > 0
``

## Let's see how many of these papers are missing citation information

In the query console window, type the following and click "run"

``select count(*) from `covid-19-dimensions-ai.data.publications
where citations_count is null
``

Can you think of a reason why it's useful to distinguish "null" citations from 0 citations?


## Let's see what the min, max, and average citation counts are

We can get multiple results in one query.

In the query console window, type the following and click "run"

``select min(citations_count) as minimum, avg(citations_count) as mean, max(citations_count) as maximum from `covid-19-dimensions-ai.data.publications``

We use aliasing with "as" to create meaningful column names in the output, instead of the default "f0_", "f1_", etc.

## Find the article with the highest citation count

In the query console window, type the following and click "run"

``select title, doi, publisher.name, year from `covid-19-dimensions-ai.data.publications`
where citations_count >= 35240
``
">=" means "greater or equal to".

## Let's create a frequency distribution of citation counts

To count how many times different citation counts appear, type the following and click "run"

``select citations_count, count(id) as n_publications from `covid-19-dimensions-ai.data.publications` 
group by citations_count
order by citations_count desc``

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

``
select year, count(id) as n from `covid-19-dimensions-ai.data.publications`
group by year
order by n desc
``

## Let's see which publishers are represented in the dataset

Note: "publisher" is a complex data field, and we use the dot-notation seen below to get just the name as a string.

type the following and click "run"

``select publisher.name, count(id) as n_publications from `covid-19-dimensions-ai.data.publications` 
group by publisher.name
order by n_publications desc``

What are the names of the top 3 publishers?


