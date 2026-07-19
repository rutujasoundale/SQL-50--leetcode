SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id ASC;

We use the SELECT DISTINCT clause to get only the unique author_id values. We want to find the unique authors who viewed at least one of their own articles.
The WHERE clause filters the rows where author_id and viewer_id are the same. This ensures that we only get the rows where authors viewed their own articles.

We use the ORDER BY clause to sort the results in ascending order based on the author_id (renamed as id in the result).

The SQL query will return the list of authors who viewed at least one of their own articles, sorted by their IDs in ascending order.

