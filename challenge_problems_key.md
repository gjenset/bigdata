# Key to challenge problems

## Find the titles and publishers of the top 5 highest cited covid-19 papers

```
select title, publisher.name from `covid-19-dimensions-ai.data.publications`
order by citations_count desc
limit 5
```

Since we order by citation counts, we can just limit to the top 5. Note that if we use the "dot" notatioin for the title, we get a slightly nicer looking ouput:

```
select title.preferred, publisher.name from `covid-19-dimensions-ai.data.publications`
order by citations_count desc
limit 5
```

## Find the publication trends

Count publicatiion IDs by year, but order by year instead of number of articles to see the trend better:

```
select year, count(id) as n from `covid-19-dimensions-ai.data.publications`
group by year
order by year asc
```

## Find the average number of citations by year

```
select year, avg(citations_count) as mean from `covid-19-dimensions-ai.data.publications`
group by year
order by year asc
```

## Find the frequency of document types in the data

```
select document_type.classification, count(id) as n from `covid-19-dimensions-ai.data.publications`
group by document_type.classification
order by n
```

## Find the number of papers that have a citation above the average

For this exercise, you get the same result by calculating the average number of citations separately and using that as a filter. However, the best way is to use a sub-query:


```
select count(*) from `covid-19-dimensions-ai.data.publications`
where citations_count > (
  select avg(citations_count) from `covid-19-dimensions-ai.data.publications`
)
```

## Find the most cited authors

Who are the most cited covid-19 researchers? This is a complex query where we need to combine a few elements:

```
select author, sum(citations_count) as n_citations  from `covid-19-dimensions-ai.data.publications`
left join unnest(researcher_ids) as author -- each researcher has a unique id
where author is not null -- avoid counting citations of papers without authors, e.g. editorials
group by author
order by n_citations desc
limit 5
```

Bonus challenge: as you can see, this query returns a list of researcher ids but no names. How could you find their actual names?
Here's a start: look for the `authors.first_name` and `authors.last_name` fields, but use the query above as a filter (in a sub-query) the researcher_ids.

## Take an in-depth look at the top papers

To get just the dois, you can use this:

```
select doi from `covid-19-dimensions-ai.data.publications`
order by citations_count desc
limit 5

```

However, instead of manually typing "doi.org/...", we can ask SQL to create the url string for us:

```
select concat("doi.org/", doi) as url_string from `covid-19-dimensions-ai.data.publications`
order by citations_count desc
limit 5

```








