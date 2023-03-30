# Challenge exercises

If you want to practice your SQL skills further, these exercises don't have the answer in the same file. Don't worry - if you want to check your query against a key, you can find it in the file "challenge_problems_key.md".

Some of these exercises are quite specific, others are more open-ended.

## Find the titles and publishers of the top 5 highest cited covid-19 papers

Can you write a query to retrieve the article title and the journal for the 5 most-cited covid-19 papers?
Hint: for this exercise, you will have to combine some syntax elements from several of the in-class exercises.

## Find the publication trends

Calculate the number of publications by year and order them so that you can clearly see a time trend in terms of numbers of publications.
Hint: there's a similar query in the in-class exercises, but you will have to change the ordering.

## Find the average number of citations by year

For each year, calculate the average number of citations for the articles published in that year. 
There's different ways of doing this: all are fine but one is more efficient than the others.

## Find the frequency of document types in the data

For this exercise, you must use the "dot" notation for the `document_type` fiel like this:

```
document_type.classification
```


## Find the number of papers that have a citation above the average

For this exercise, count the number of papers, filtering by citation score.

## Find the most cited authors

Since each article can have multiple authors, you will need to follow the `left join unnest` example from the in-class exercises, and to aggregate (`sum`) their citations over all their papers.


## Take an in-depth look at the top papers

For this exercise, you need find the doi (digital object identifier) in the top 3 most cited papers. If one of them is missing a doi, take the next one on the list that has one.

Once you have the dois, you can find the original article by opening a browser tab, and writing:

```
http://doi.org/
```

followed by the doi. Complete example:

```
http://doi.org/10.1001/jama.2020.2648
```
