# reddit-mean-score

R + ggplot2 code for a very quick data visualization for [Reddit Mean Submission Score by Subreddit](https://www.reddit.com/r/dataisbeautiful/comments/duub8p/average_reddit_submission_score_by_title_length/)

The data was retrieved from BigQuery via this query:

```sql
#standardSQL
WITH
  top_subreddits AS (
  SELECT
    subreddit
  FROM
    `fh-bigquery.reddit_posts.*`
  WHERE
    _TABLE_SUFFIX BETWEEN '2019_01' AND '2019_08'
    AND score >= 5
    AND LENGTH(title) >= 8
  GROUP BY
    subreddit
  HAVING
    COUNT(*) >= 2000
  ORDER BY
    APPROX_COUNT_DISTINCT(author) DESC
  LIMIT
    50 )

SELECT
  subreddit,
  LENGTH(title) as title_length,
  AVG(score) as avg_score
FROM
  `fh-bigquery.reddit_posts.*`
WHERE
  _TABLE_SUFFIX BETWEEN '2017_01' AND '2019_08'
  AND LENGTH(title) <= 300
  AND subreddit IN (select subreddit from top_subreddits)
GROUP BY subreddit, title_length
ORDER BY subreddit, title_length
```

## Maintainer/Creator

Max Woolf ([@minimaxir](https://minimaxir.com))

*Max's open-source projects are supported by his [Patreon](https://www.patreon.com/minimaxir). If you found this project helpful, any monetary contributions to the Patreon are appreciated and will be put to good creative use.*

## License

MIT
