# Analysis of PyPI downloads by distro

From [BigQuery query](https://bigquery.cloud.google.com/savedquery/411151289586:9ceba9940d5e429da113851b07eb38f7):

```{sql}
details.distro.id as distro_id
FROM `the-psf.pypi.downloads*`
WHERE file.project = 'numpy'
  AND file.type = 'bdist_wheel'
  -- Only query the last 30 days of history
  AND _TABLE_SUFFIX
    BETWEEN FORMAT_DATE(
      '%Y%m%d', DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY))
    AND FORMAT_DATE('%Y%m%d', CURRENT_DATE())
 GROUP BY distro_name, distro_version, distro_id
 ORDER BY downloads DESC
```

Last updated 7 August 2019.
